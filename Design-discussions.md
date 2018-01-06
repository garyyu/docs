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

# Understanding kernels

@dmdeemer pasted https://pastebin.com/aCdznJW1 and there was a Lobby discussion with @tromp

Q: Is it necessary for all nodes to store the set of tx kernels?

A: They're needed for new full nodes to sync, so archival full nodes must store them.

Q: Do we need to store all kernels, forever?

A: ....yes.... they are necessary for a new client to validate the chain.

JT: If the whole history is A->B with kernel kAB and B->C (same amount, i.e. 0 change) with kernel kBC then you cannot forget kAB, even though there's only 1 UTXO. Only the sum of kAB and kBC proves 0-inflation. B and C cannot sign for kAB+kBC, that would still need A.

JT: Excess values combine blinding factors from different parties, and therefore need both parties to sign. Excess values must relate inputs and outputs in order to prove 0 inflation. Specifically, they prove that sum outputs minus sum inputs is a commitment to 0.

D: Ok, I need a concrete example...
````
Alice has 1 Grin. She sends it to Bob

Half-transaction:
(b1*G + 1*H) + (e1*G) = ...

Bob needs to know the amount, 1 Grin, and the sum 
of the blinding values (b1+e1)  Alice can tell 
him that, because she remembers b1 and e1.

Bob then picks b2 and e2, such that b1+e1 = b2+e2,
and completes the transaction:

(b1*G + 1*H) + (e1*G) = (b2*G + 1*H) + (e2*G)
````

D: And now I see the problem... Alice just told bob exactly the opening information Bob will later need to spend his output. D'oh!

JT: It's not safe for parties to tell each other blinding factors. See the mailing list discussion for how to transact properly. Specifically https://lists.launchpad.net/mimblewimble/msg00087.html and the later https://lists.launchpad.net/mimblewimble/msg00091.html

# Grin dev discussion about #215 (the lock_height switch commit stuff)

To land mimblewimble/grin#215, the plan is:

 * Extend wallet (for now) to store a Merkle proof.
   (for starters it's simpler to store these in the wallet, before we endpoints in place. And recreating them from scratch could be made part of `wallet recover`)
 * Remove both get_output_by_commit and get_block_header_by_output_commit (NOT remove sumtrees/utxo :grinning:)
 * The only commitment or output index is what the sumtrees offer. Any additional information needs to be provided to spend.
 * (No new index needed to map commitment|block_hash -> pmmr pos. We can lookup the block with that hash)
 * Only full (archival) nodes can surely know if a coinbase output is locked or not (1000 confirms)
 * Wallet recovery still works.
 * Miners must remember which output they mined where, or rely on `wallet recover`

The changes to the pmmr to maintain switch commits should be fairly small

For the output pmmr on a non-archival node, instead of a block hash, we'll require a Merkle proof.
	 (The path has a list of hashes in the MMR from the output to the root.)
	 This is going to be large. We can figure out more succinct encoding later.
	 Output PMMR looks the exact same, but you don't have any older block data.
	 The output hash is first on the path, and the root which is at the end of the path is in the block header,
	 so the path can't be falsified.

We save the roots (which we recalculate for every block) on the block header.
The Merkle proof proves that the output is in that block header.
We might need an index from UTXO root to block hash but that's good to have anyway.
If I send you a UTXO set as of block 1000, the roots tie the headers to what I sent you, and the headers tie up proof-of-work not for inclusion no, but the roots will be checked by fast sync.

