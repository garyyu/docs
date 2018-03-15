
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

## Mining
1. Make sure you're synched up to the tip of the chain (Testnet1 - later on this will be more automatic...) by running `grin server run` and wait 1-2 hours or so until you reach the tip of chain and see the debug message "Disabling sync"
2. Look in the `target/debug/plugins` you find any mining plugings that have been built. Current fastest plugin is called lean or mean or so, and runs best on modern (2015+) Intel CPUs. Update grin.toml to use your preferred mining plugin, maybe by commenting _compat plugin and uncommenting the other faster plugin.
3. Run `grin wallet listen &` after again ensuring 1. above. The listen is needed so newly mined coinbase coins can be safely stored.
4. Run `grin server -m run`

Be warned - you'll be mining test coins (no value) and there are bugs all over the place. Help us squish them! First see  [troubleshooting Testnet1](https://github.com/mimblewimble/docs/wiki/Testnet1-troubleshooting) for the easy ones, and then come over to the [Grin Lobby](https://gitter.im/grin_community/Lobby) and talk about your bug collection!