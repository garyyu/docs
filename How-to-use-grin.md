
## First time preparations

Create some folders and setup PATH:

````bash
cd grin # if you're not already here. This is the folder that `git clone` created for you.
export PATH=`pwd`/target/debug:$PATH # Redo in every new window, or: cargo install
cd ..
mkdir wallet
cd wallet
grin wallet init
cd ..
grin help # Shows all grin subcommands
````

## Configuration

Configure grin using `grin.toml` and on the command line.

At startup, grin looks for a configuration file called 'grin.toml' in the following places in the following order, using the first one it finds:

1. The current working directory
2. The directory in which the grin executable is located
3. {USER_HOME}/.grin

Command line switches can be used to override grin.toml settings.

If no configuration file is found, command line switches must be given to grin in order to start it. If a configuration file is found but no command line switches are provided, grin starts in server mode using the values found in the configuration file.

At present, the relevant modes of operation are 'server' and 'wallet'. When running in server mode, any command line switches provided will override the values found in the configuration file. Running in wallet mode does not currently use any values from the configuration file other than logging output parameters.

## Participate on Testnet1

Ask about errors you see, and try retelling the answers in your own words.
That brings attention to them, so we can make them more understandable for
users, and improve the page about
[troubleshooting](https://github.com/mimblewimble/docs/wiki/Troubleshooting).

As a user, you can try to:

- ☑ sync the chain
  - ☑ fast sync
  - ☑ headers-first sync
- ☑ mine, using any of these PoW implementations:
  - ☑ cpu_mean_compat mining plugin (typical performance: 1000 graphs/s)
  - ☑ cpu_mean mining plugin (typical performance: 2000 graphs/s)
  - ☐ GPU miner
- ☑ view your wallets "outputs"
  - BUG: spendable outputs might be hidden or forgotten.
    WORKAROUND: Backup, then edit wallet.dat manually.
    A wallet reconstruction command is in development
    [#295](https://github.com/mimblewimble/grin/issues/295)
  - BUG: when chain sync fails, coins might be incorrectly marked "Spent".
    WORKAROUND: Search-replace "Spent" with "Unconfirmed" and resync
  - BUG: when you activate mining, outputs are created that can be shown as
    having a value (50.0) and just awaiting confirmation; but if you don't get
    at least 1 confirmation within 1-5 minutes you can be sure they never will.
    WORKAROUND: wait until chain can fully sync before you start mining, and
    then babysit your mining process in case it forks off
- ☑ send transactions
  - The simple transaction format is Testnet1 only.
  - The new transaction creates transactions interactively.
  - BUG: after you create a transaction, the outputs it consumes are Locked
    until the transaction confirms.
    WORKAROUND: create transactions on the command line, use `-s smallest`
    and if your recipient fails to claim in a reasonable time, you can always
    claim it back yourself, loosing only the fee. Or do a full resync...

Legend for the above: `☑ Probably yes now, or ☐ Probably not now.`

## Known problems
When you run into problems, please see [troubleshooting](https://github.com/mimblewimble/docs/wiki/Troubleshooting)
and ask on [gitter chat](https://gitter.im/grin_community/Lobby).

Especially if you're not sure you've found a new bug,
[please ask on the chat](https://gitter.im/grin_community/Lobby)
or on the [maillist](https://launchpad.net/~mimblewimble)

And before you file a new bug, please take a quick look through
[known bugs](https://github.com/mimblewimble/grin/issues?utf8=%E2%9C%93&q=label%3Abug+)
and other existing issues.

## Mining
1. Make sure you're synched up to the tip of the chain (Testnet1 - later on this will be more automatic...) by running `grin server run` and wait 1-2 hours or so until you reach the tip of chain and see the debug message "Disabling sync"
2. Look in the `target/debug/plugins` you find any mining plugings that have been built. Current fastest plugin is called lean or mean or so, and runs best on modern (2015+) Intel CPUs. Update grin.toml to use your preferred mining plugin, maybe by commenting _compat plugin and uncommenting the other faster plugin.
3. Run `grin wallet listen &` after again ensuring 1. above. The listen is needed so newly mined coinbase coins can be safely stored.
4. Run `grin server -m run`

Be warned - you'll be mining test coins (no value) and there are bugs all over the place. Help us squish them! First see  [troubleshooting Testnet1](https://github.com/mimblewimble/docs/wiki/Testnet1-troubleshooting) for the easy ones, and then come over to the [Grin Lobby](https://gitter.im/grin_community/Lobby) and talk about your bug collection!