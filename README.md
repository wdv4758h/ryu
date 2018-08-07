# Ryū [![Build Status](https://travis-ci.org/dtolnay/ryu.svg?branch=master)](https://travis-ci.org/dtolnay/ryu)

Pure Rust implementation of Ryū, an algorithm to quickly convert floating point
numbers to decimal strings.

The PLDI'18 paper [*Ryū: fast float-to-string conversion*][paper] by Ulf Adams
includes a complete correctness proof of the algorithm. The paper is available
under the creative commons CC-BY-SA license.

This Rust implementation is a line-by-line port of Ulf Adams' implementation in
C, [https://github.com/ulfjack/ryu][upstream]. The `ryu::raw` module exposes
exactly the API and formatting of the C implementation as unsafe pure Rust
functions. There is additionally a safe API as demonstrated in the example code
below. The safe API uses the same underlying Ryū algorithm but diverges from the
formatting of the C implementation to produce more human-readable output, for
example `0.3` rather than `3E-1`.

*Requirements: this crate supports any compiler version back to rustc 1.15; it
uses nothing from the Rust standard library so is usable from no_std crates.*

[paper]: https://dl.acm.org/citation.cfm?id=3192369
[upstream]: https://github.com/ulfjack/ryu/tree/2936e610f0909b036e857a43ded70e064f7cfc7f

```toml
[dependencies]
# not yet published to crates.io
ryu = { git = "https://github.com/dtolnay/ryu" }
```

## Examples

```rust
extern crate ryu;

fn main() {
    let mut buffer = ryu::Buffer::new();
    let printed = buffer.format(1.234);
    assert_eq!(printed, "1.234");
}
```

## Performance

You can run upstream's benchmarks with:

```console
$ git clone https://github.com/ulfjack/ryu c-ryu
$ cd c-ryu
$ bazel run -c opt //ryu/benchmark
```

And our benchmarks with:

```console
$ git clone https://github.com/ulfjack/ryu rust-ryu
$ cd rust-ryu
$ cargo run --example benchmark --release
```

These benchmarks measure the average time to print a 32-bit float and average
time to print a 64-bit float, where the inputs are distributed as uniform random
bit patterns 32 and 64 bits wide.

The upstream C code, the unsafe direct Rust port, and the safe pretty Rust API
all perform the same, taking around 21 nanoseconds to format a 32-bit float and
31 nanoseconds to format a 64-bit float.

## License

Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
