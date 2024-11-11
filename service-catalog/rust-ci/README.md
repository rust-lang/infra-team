# Rust CI

The Infrastructure team maintains the [rust-lang/rust] Continuous Integration,
which lives in the [workflows] directory.

The [terraform/rust-ci] module provisions some resources used by the CI:

- The `rust-lang-ci-sccache2` S3 bucket, used to store [sscache] compilation
  artifacts to speed up the rustc compile time and served by Cloudfront CDN at
  `ci-caches.rust-lang.org`.
- The `rust-lang-ci2` S3 bucket, used to store compilation artifacts and served
  by Cloudfront CDN at `ci-artifacts.rust-lang.org`.

[sscache]: https://github.com/mozilla/sccache
[terraform/rust-ci]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/rustc-ci
[rust-lang/rust]: https://github.com/rust-lang/rust/
[workflows]: https://github.com/rust-lang/rust/tree/master/.github/workflows
