# Cross-compiling

It should be fairly easy and cheap to build and run a validating node everywhere (wherever Rust can run). That being said, it is possible to cross-compile `grin` on a x86 Linux platform and produce ARM binaries, for example, in order to run on a Raspberry Pi. You will need to use the `milestone/testnet1` branch and uncomment the `no-plugin-build` feature as documented [below](#cuckoo-miner-considerations) to avoid compiling and running the mining plugins.

If you have any issues with native cross-compilation in your host system, you can try using Docker and [rust-on-raspberry-docker](https://github.com/kargakis/rust-on-raspberry-docker).

## Cuckoo-Miner considerations

If you're having issues with building cuckoo-miner plugins (which will usually manifest as a lot of C errors when building the `grin_pow` crate, you can turn mining plugin builds off by editing the file `pow/Cargo.toml' as follows:

```toml
# uncomment this feature to turn off plugin builds
features=["no-plugin-build"]
```

This may help when building on 32 bit systems or non x86 architectures. You can still use the internal miner to mine by setting:

```toml
use_cuckoo_miner = false
```

in `grin.toml`
