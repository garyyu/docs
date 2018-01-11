# Errors from grin commands

## Wallet: Coins are 'confirmed but still locked'?
Like other cryptocurrencies, newly mined coins are time locked, so mined coins can't be spent immediately.  The lock time, a.k.a. COINBASE_MATURITY, is set to 1000 blocks on testnet1.

## Wallet: What's my IP:port to test receiving coins?
This is your wallet base URL, something like http://1.2.3.4:13415
NOTE: Run `grin wallet -e listen` to make your wallet reachable from outside.

## Server: "Peer request error" or other peer/network issues after restarting grin server
Possible workaround is rm -rf .grin/peers/*  then restart.

## Server: grin server or wallet crashes or hangs
Yes, this still happens quite often. You'll need to babysit grin.
Very welcome any solutions to give grin a "watchdog" solution that can restart
grin in case of trouble.


# Build errors

## Build error: Could not compile `tokio-retry`.
You might want to remove any previous rust installations to avoid conflicts.
Use `rustup` to [reinstall rust and cargo as described](build.md).

NOTE: If you install rust or cargo with your package manager (most Linuxes
anno 2017) you'll get too old versions. On Debian, you might have to manually
compile cmake or get it from non-detault repositories.

## Build error: `failed to select a version for 'serde_json'`
Run `cargo update` to fix this

## Build error: Alpine Linux build error, something `rocksdb` and upon digging deeper; rocksdb/env/env_posix.cc:13:22: fatal error: linux/fs.h: No such file or directory
On Alpine Linux, grin build requires kernel headers. After `apk add linux-headers` grin builds ok.

## Build error: can't compile crate `bitflags``
Chech `rustc --version` and note that bitflags requires rust 1.21 or newer. Install via `rustup` and recommended you also remove any rust/cargo installed via your package manager.

## Build error: librocksdb-sys 5.7.1+ / rocksdb ^0.8.0 won't build
You need more llvm and clang.
On Fedora 24/25 `dnf install llvm-devel clang-devel`.
On apt-get you'll need llvm-dev libclang-dev clang.
On Arch you need clang and clanlib (clanglib?)
On CentOS 7 https://stackoverflow.com/questions/44219158/how-to-install-clang-and-llvm-3-9-on-centos-7
More details in https://github.com/mimblewimble/grin/issues/584
