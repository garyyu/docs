# Troubleshooting

## Wallet: Coins are 'confirmed but still locked'?
Like other cryptocurrencies, newly mined coins are time locked, so mined coins can't be spent immediately.

## Wallet: What's my IP:port to test receiving coins?
This is your wallet base URL, something like http://1.2.3.4:13415
NOTE: Run `grin wallet -e listen` to make your wallet reachable from outside.

## Server: "Peer request error" or other peer/network issues after restarting grin server
Possible workaround is rm -rf .grin/peers/*  then restart.

## Server: grin server or waller crashes or hangs
Yes, this still happens quite often. You'll need to babysit grin.
Very welcome any solutions to give grin a "watchdog" solution that can restart
grin in case of trouble.

## Build error: Could not compile `tokio-retry`.
You might want to remove any previous rust installations to avoid conflicts.
Use `rustup` to [reinstall rust and cargo as described](build.md).

NOTE: If you install rust or cargo with your package manager (most Linuxes
anno 2017) you'll get too old versions. On Debian, you might have to manually
compile cmake or get it from non-detault repositories.

## Build error: `failed to select a version for 'serde_json'`
Run `cargo update` to fix this
