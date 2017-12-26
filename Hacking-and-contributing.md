# Hacking
 * git checkout master; git pull; git checkout -b feature/my_branch
 * change code
 * add tests for new code
 * `cargo test --all`
 * All good? make +rustfmt happy, then submit a Pull Request on github (google for good guides on how github PR's work)

And why not try running your own little grin network? See the [local net documentation](https://github.com/mimblewimble/grin/blob/master/doc/local_net.md)

# Testing

## Preparations

````
cargo install cargo-check
cargo install cargo-cov
cargo install cargo-tarpaulin
````

## Run
Run a quick syntax check with `cargo check`
Run the full test suite with `cargo test --all`
Run a test coverage analysis with `cargo cov test`

### Troubleshooting
 * Q: `cargo cov test` gives error `error: Native profiler library not found`
 A: Install clang with `sudo apt-get install clang` or similar.
