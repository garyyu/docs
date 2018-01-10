
`lock height`
  For coinbase coins (outputs), they must be locked for a while (1000 confirmations or around 17 hours) or else chain reorganisations will cause a lot of trouble.

`mmr`
  Merkle Mountain Range

`pmmr`
  Pruneable Merkle Mountain Range

`output` [(defined here)](https://github.com/mimblewimble/grin/blob/master/core/src/core/transaction.rs#L479)
	OutputFeatures - currently a Boolean == coinbase or not ("Options for an output's structure or use")
	Commitment - `rG+vH` The homomorphic commitment representing the output's amount
	SwitchCommitHash - `blake2(rJ)` The switch commitment hash, a 160 bit length blake2 hash of blind*J
	RangeProof - A proof that the commitment is in the right range

`input` [(defined here)](https://github.com/mimblewimble/grin/blob/master/core/src/core/transaction.rs#L375)
	A reference to an output being spent by a transaction.

`switch commitment`
      a hash of something, like the blinding pubkey

`grins`, `milligrins`
  used to denominate coins (chosen by popular vote on the mailing list)

`kernel`
  the core piece of a transaction, and one that must be kept also when transactions are merged

`pre-image`
    something, like a blinding pubkey?

### Elliptic algebra

`r`
  a blind (like a pubkey?)

`rG+vH`
  a homomorphic commitment

`rJ`
  switch commitment

`Bj`

