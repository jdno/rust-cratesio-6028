# Minimal Complete Verifiable Example for rust-lang/crates.io#6028

This repository implements a verifiable example for the issue reported in
<https://github.com/rust-lang/crates.io/issues/6028>. It is used to reproduce
the issue on staging and confirm a fix.

The root cause of <https://github.com/rust-lang/crates.io/issues/6028> is that
crates larger than 20MB cannot be served with Fastly[^1] without enabling
[segmented caching](https://docs.fastly.com/en/guides/segmented-caching).
Without this feature, the CDN returns a `503 Response object too large` error as
a response.

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or <http://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or <http://opensource.org/licenses/MIT>)

at your option.

[^1]: https://docs.fastly.com/en/guides/failure-modes-with-large-objects
