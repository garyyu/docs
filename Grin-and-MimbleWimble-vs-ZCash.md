# Compare MimbleWimble/Grin and ZCash
> Posted on **2016-11-01 06:08:52**    Original author: **Ignotus Peverell**

The comparison between Grin as an implementation and MimbleWimble as its core protocol and ZCash are natural as they fit in a similar space. We try to address most comparison points here, hopefully without too much bias. Note that for now, MimbleWimble isn't implemented anywhere and Grin is far from ready. Until we have a first stable release, ZCash wins everywhere and the following is just conjectures.

### Grin Wins

- No trusted setup required (or any kind of setup for that matter, other than choosing a genesis block).
- Excellent asymptotic scaling and practical scaling. Grin scales with the UTXO set and each UTXO can be made tiny after some time (as the rangeproof can be eventually dropped).
- Building transactions and verifying them in MimbleWimble is trivial computationally and can easily be done on smartphones or raspberry pies. On the other hand, at the time of this writing, building ZCash blinded transactions requires around 4GB of memory and about a minute of computation.
- All transactions in MimbleWimble are obscured by default while ZCash transparent transactions seem to be the majority of transactions at this point.
- Only relies on simple and very well vetted cryptographic constructs and assumptions.
- Green field implementation that strives to be as clear and simple as possible, making it easier to audit and maintain in the future.
 - Grin is a community driven implementation with no "Founders Reward".

### ZCash Wins

- MimbleWimble does not support scripting. While some features that were introduced in Bitcoin through scripting can still exist in MimbleWimle (like multisig and time locks), the absence of generalized scripting is more limited. Note that at this time ZCash does not support generalized scripting either but there is no theoretical reason why it couldn't.
- While Grin transaction outputs are fully obscured, it's still possible to trace which inputs links to which outputs at least until some age. But it's unclear at this point what information could be derived from this.
- Implementation based on a fork of Bitcoin Core, which is a very mature (albeit hard to maintain) codebase.
- ZCash is backed by a funded company.

### Related Discussions

https://www.reddit.com/r/Mimblewimble/comments/59qulw/mimblewimble_vs_zcash/
