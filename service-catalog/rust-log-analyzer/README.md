# Rust Log Analyzer

[rust-log-analyzer] is a tool that analyzes the CI build logs of the [rust]
repository to automatically extract error messages from failed builds.

This tool is deployed via the [terraform/rust-log-analyzer] module as an ECS
service and uses S3 to store the GitHub actions index.

## Versions

These are the versions we need to keep up-to-date:

- Ubuntu version in the [Dockerfile]
- Dependencies in [Cargo.lock]
- GitHub Actions versions in the [workflows]

[Cargo.lock]: https://github.com/rust-lang/rust-log-analyzer/blob/master/Cargo.lock
[Dockerfile]: https://github.com/rust-lang/rust-log-analyzer/blob/master/Dockerfile
[rust-log-analyzer]: https://github.com/rust-lang/rust-log-analyzer
[rust]: https://github.com/rust-lang/rust
[terraform/rust-log-analyzer]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/rust-log-analyzer
[workflows]: https://github.com/rust-lang/rust-log-analyzer/tree/master/.github/workflows
