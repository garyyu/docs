# Requirements
Before building, [check requirements](https://github.com/mimblewimble/docs/wiki/Requirements)

# Build steps

## Preparation

Run this in a command line

```sh
git clone https://github.com/mimblewimble/grin.git -b milestone/testnet1
cd grin
```

And make sure you're on the milestone/testnet1 branch if you're testing on Testnet1 *only*.

NOTE: Testnet1 is more or less completed and soon no longer active, so [see the wiki](https://github.com/mimblewimble/docs/wiki) for newest information on when & how you can join later Testnets.

## Building
 1. Run `cargo build`
 2. Seeing red messages and errors? See [troubleshooting Testnet1](https://github.com/mimblewimble/docs/wiki/Testnet1-troubleshooting)
 3. Build may seem to get stuck, but it probably just takes time. In the end you get a `target/debug/grin` binary that you use when you run grin.

Building locally to deploy on another platform (VPS, embedded, ...)? Then read about [cross-compiling](https://github.com/mimblewimble/docs/wiki/More-on-building)

# Using grin

See [usage](https://github.com/mimblewimble/docs/wiki/Usage)