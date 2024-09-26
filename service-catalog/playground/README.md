# Rust Playground

The [Rust Playground] is a service that allows users to run Rust code in a web
browser. More info in the Playground [help page].

[terraform/playground] deploys the Rust Playground infrastructure.

The Playground is deployed in an AWS EC2. The [deployment]
directory contains information and scripts on how to deploy it.

## Versions

- Ubuntu version of the EC2 instance.
- [package.json](https://github.com/rust-lang/rust-playground/blob/main/ui/frontend/package.json).
- [Cargo.lock](https://github.com/rust-lang/rust-playground/blob/main/ui/Cargo.lock).

[Rust Playground]: https://play.rust-lang.org/
[help page]: https://play.rust-lang.org/help
[terraform/playground]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/playground
[deployment]: https://github.com/rust-lang/rust-playground/tree/main/deployment
