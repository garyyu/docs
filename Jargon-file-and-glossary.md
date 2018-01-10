# Jargon file and glossary

`lock height`
  For coinbase coins (outputs), they must be locked for a while (grin 1000/60 = ~15hours?) or else chain are near impossible.

`mmr`
  Merkle Mountain Range

`pmmr`
  Pruneable Merkle Mountain Range

`output`
	OutputFeatures - currently a Boolean == coinbase or not ("Options for an output's structure or use")
	Commitment - `rG+vH` The homomorphic commitment representing the output's amount
	SwitchCommitHash - `blake2(rJ)` The switch commitment hash, a 160 bit length blake2 hash of blind*J
	RangeProof - A proof that the commitment is in the right range
	- from https://github.com/mimblewimble/grin/blob/master/core/src/core/transaction.rs#L479

`input`
	A reference to an output being spent by a transaction
  - from https://github.com/mimblewimble/grin/blob/master/core/src/core/transaction.rs#L375



`switch commitment`
      a hash of something, like the blinding pubkey

`pre-image`
    something, like a blinding pubkey?