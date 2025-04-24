# CI Mirrors

The Rust Project mirrors some files in `ci-mirrors.rust-lang.org`, which is a
[CDN](https://github.com/rust-lang/simpleinfra/blob/30a701eebbf0dea1fcd9a49104d9b9908a9929cc/terraform/shared/static-websites.tf#L9) serving
the contents of the
[rust-lang-ci-mirrors](https://github.com/rust-lang/simpleinfra/blob/30a701eebbf0dea1fcd9a49104d9b9908a9929cc/terraform/shared/s3.tf#L69)
S3 bucket.

The main benefit of mirroring files is reliability: our CDN is very stable, and
it might provide a better uptime than the original source.

## How to mirror a new file

To mirror a new file, add it to the
[ci-mirrors](https://github.com/rust-lang/ci-mirrors/tree/main/files)
repository.
