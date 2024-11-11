# Releases

Here's the infrastructure we use to distribute the Rust releases:

- [release-distribution](https://github.com/rust-lang/simpleinfra/blob/master/terragrunt/modules/release-distribution/):
  Terraform module that uses Cloudfront and Fastly to manage the distribution of
  releases via `static-rust-lang-org`.
- [releases](https://github.com/rust-lang/simpleinfra/blob/docs-document-release-distribution/terraform/releases/):
  Terraform module managing the infrastructure that publishes Rust releases,
  including [promote-release](https://github.com/rust-lang/promote-release),
  the tool used to publish new releases of the Rust toolchain.

## Versions

`simpleinfra` repo:

- Terraform providers

`promote-releases` repo:

- [Cargo.toml](https://github.com/rust-lang/promote-release/blob/master/Cargo.toml)
  dependencies
- GitHub Actions: specified in the
  [workflows](https://github.com/rust-lang/promote-release/tree/master/.github/workflows)
  directory
- Ubuntu version specified in the
  [Dockerfile](https://github.com/rust-lang/promote-release/blob/master/prod/Dockerfile)
