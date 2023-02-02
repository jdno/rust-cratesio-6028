# Minimal Complete Verifiable Example for rust-lang/crates.io#6028

This repository implements a verifiable example for the issue reported in
<https://github.com/rust-lang/crates.io/issues/6028>. It is used to reproduce
the issue on staging and confirm a fix.

The root cause of <https://github.com/rust-lang/crates.io/issues/6028> is that
crates larger than 20MB cannot be served with Fastly[^1] without enabling
[segmented caching](https://docs.fastly.com/en/guides/segmented-caching).
Without this feature, the CDN returns a `503 Response object too large` error as
a response.

## Usage

The issue can be reproduced by sending the following request:

```shell
curl --request GET \
  --url https://fastly-static.staging.crates.io/crates/rust-cratesio-6028/rust-cratesio-6028-0.1.0.crate \
```

This will return the following error:

```html
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
 "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html>
  <head>
    <title>503 Response object too large</title>
  </head>
  <body>
    <h1>Error 503 Response object too large</h1>
    <p>Response object too large</p>
    <h3>Error 54113</h3>
    <p>Details: cache-ams12780-AMS 1675336948 3776809559</p>
    <hr>
    <p>Varnish cache server</p>
  </body>
</html>
```

## Generate File

The `data.bin` file in the crate is used to increase the size of the crate past
the limit for Fastly. It can be set to an arbitrary size with the following
command:

```shell
dd if=/dev/zero of=output.dat  bs=1m  count=24
```

`count` controls how many blocks of size `bs` are written to the file. In the
above example, the file is set to 24MB.

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or <http://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or <http://opensource.org/licenses/MIT>)

at your option.

[^1]: https://docs.fastly.com/en/guides/failure-modes-with-large-objects
