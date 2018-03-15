To build the grin application from source, you need:

 - Linux or Mac OS
   - Using Windows? Then run on Linux in a VM or [help add windows compatibility](https://github.com/mimblewimble/docs/wiki/Hacking-and-contributing)
 - `cmake --version` # 3.2 or newer
   - Debian stable? You might have to compile cmake from source.
 - `rustc --version` # 1.21 or newer
   - NOTE: Install via [Rustup](https://www.rustup.rs/) as `curl https://sh.rustup.rs -sSf | sh; source $HOME/.cargo/env` and **avoid your package manager's rust** for now, trust us.
 - 2 GB RAM to compile
   - Either [compile locally](https://github.com/mimblewimble/docs/wiki/More-on-building) and deploy on a memory-limited computer.
   - or the simpler, maybe slower option, to build on a 1GB VPS, do:
````
free -h # verify you DON'T already have enough swap
# No GB of swap? Then continue
sudo dd if=/dev/zero of=/swapfile bs=1M count=2048
sudo mkswap /swapfile
sudo swapon /swapfile
free -h # verify; you should now have 2GB swap; and you can go ahead: cargo build
````
