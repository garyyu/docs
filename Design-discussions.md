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

