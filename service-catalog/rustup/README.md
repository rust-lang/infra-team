# Rustup

Rustup is the recommended tool for installing Rust.
It is a command line tool that manages Rust versions and associated tools.

The Infra team maintains the infrastructure for Rustup:

- [rustup](https://github.com/rust-lang/simpleinfra/tree/master/terragrunt/modules/rustup)
  terragrunt module.
  An S3 bucket called `rustup-build` for storing the Rustup binaries,
  distributed through CloudFront CDN at `rustup-builds.rust-lang.org`.
  The [rust-lang/rustup](https://github.com/rust-lang/rustup)
  GitHub repository builds and uploads Rustup artifacts
  to this S3 bucket.
- [win-rustup-rs](https://github.com/rust-lang/simpleinfra/tree/master/terragrunt/modules/win-rustup-rs)
  terragrunt module.
  A Cloudfront distribution of the `/rustup/dist` path of the
  `static-rust-lang-org` bucket providing convenient short for downloading
  rustup on Windows.
