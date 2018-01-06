# Current state of Grin's data stores

The db_root = .grin setting in grin.toml decides where grin stores its big
and small data.

The chain is the biggest in size. It's stored in a key-value database
using Rocksdb (like Leveldb) and has the following contents:

````
.grin/chain - handled by chain::store (key prefix: "chain")
	"h" + hash -> block header
	"b" + hash -> block
	"H" + hash -> head
	"I" + ? -> header head
	"s" + ? -> sync head
	"8" + ? -> header height
	"o" + ... -> output by commit  -- to be deprecated
	"p" + ... -> header by output  -- to be deprecated
	"c" + ? -> commit pos
	"k" + ? -> kernel pos
````
A grin node that does not archive the full chain can prune a lot of this data.

There is also a small rocksdb listing all known peers:
````
.grin/peers - handled by peer::store (prefix: "peers")
	"p" + some id -> serde-serialized peer (address, status, ban timeout)
````
The PMMR data structure is used for several things, and the biggest
in size is utxo - the full list of unspent coins.
````
sumtrees/kernel  Just an append-only Merklish tree, neither sum nor pruning are used.
sumtrees/rangeproof  The PMMR is just a prunable tree. The summation isn't used.
sumtrees/utxo Summation, Merklish tree of all valid UTXOs
````


# Grin dev discussion about #215 (the lock_height switch commit stuff)

To land mimblewimble/grin#215, the plan is:
 • Extend wallet (for now) to store a Merkle proof.
   (for starters it's simpler to store these in the wallet, before we endpoints in place. And recreating them from scratch could be made part of `wallet recover`)
 • Remove both get_output_by_commit and get_block_header_by_output_commit (NOT remove sumtrees/utxo :grinning:)
 • The only commitment or output index is what the sumtrees offer. Any additional information needs to be provided to spend.
 • (No new index needed to map commitment|block_hash -> pmmr pos. We can lookup the block with that hash)
 • Only full (archival) nodes can surely know if a coinbase output is locked or not (1000 confirms)
 • Wallet recovery still works.
 • Miners must remember which output they mined where, or rely on `wallet recover`
 • The changes to the pmmr to maintain switch commits should be fairly small
 • For the output pmmr on a non-archival node,
   instead of a block hash, we'll require a Merkle proof.
	 (The path has a list of hashes in the MMR from the output to the root.)
	 This is going to be large. We can figure out more succinct encoding later.
	 Output PMMR looks the exact same, but you don't have any older block data.
	 The output hash is first on the path, and the root which is at the end of the path is in the block header,
	 so the path can't be falsified.

We save the roots (which we recalculate for every block) on the block header.
The Merkle proof proves that the output is in that block header.
We might need an index from UTXO root to block hash but that's good to have anyway.
If I send you a UTXO set as of block 1000, the roots tie the headers to what I sent you, and the headers tie up proof-of-work not for inclusion no, but the roots will be checked by fast sync.

