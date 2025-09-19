# Developing

Rotel depends on the latest Rust toolchain, we recommend [rustup](https://rustup.rs/). 

# Additional Dependencies
* gcc or another compiler with linker for libc
* cmake
* openssl
* protoc
* libzstd-dev

## Building and running

To build:
```shell
cargo build
```

To run:
```shell
cargo run start <opts>
```

Add the `--release` flag to build a release version.

### Custom Allocators

Rotel supports optional custom memory allocators to improve performance. You can choose to use one of these allocators or use the default system allocator.

#### Using jemalloc

jemalloc is a general-purpose malloc implementation that emphasizes fragmentation avoidance and scalable concurrency support:

```shell
cargo build --features jemalloc --release
```

This works well for most platforms, particularly x86_64 targets (both glibc and musl):

```shell
cargo build --target x86_64-unknown-linux-musl --features jemalloc --release
```

Note: jemalloc has known compatibility issues with aarch64-musl targets. For those platforms, consider using mimalloc instead.

#### Using mimalloc

mimalloc is a compact general-purpose allocator with excellent performance developed by Microsoft:

```shell
cargo build --features mimalloc --release
```

This can be a good alternative when jemalloc has compatibility issues:

```shell
cargo build --target aarch64-unknown-linux-musl --features mimalloc --release
```

#### Important Note

You can only enable one allocator at a time. If you try to enable both, you'll get a compile error.

If you encounter build issues with custom allocators on any platform, you can always build without these features to use the default system allocator.

## Running tests

```shell
cargo test
```

## Profiling

Profiling with pprof is not enabled by default, you must build with the cargo feature `pprof`. 

When you have rebuilt with pprof support, the following options are available. You can set at most one.

* `--pprof-flame-graph`
* `--pprof-call-graph`

