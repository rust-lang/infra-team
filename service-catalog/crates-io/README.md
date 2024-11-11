# Crates.io

Crates.io is the Rust community's package registry. The crates.io
[team](https://www.rust-lang.org/governance/teams/dev-tools#team-crates-io)
develops [crates-io](https://github.com/rust-lang/crates.io) and the Infra team
helps with the infrastructure.

The crates.io app is deployed in Heroku, you can learn more about it in the
crates-io [docs](https://github.com/rust-lang/crates.io/blob/main/docs/ARCHITECTURE.md).

Here are the details of the crates.io infrastructure managed by the infra team:

- [crates-io-logs](https://github.com/rust-lang/simpleinfra/tree/master/terragrunt/modules/crates-io-logs):
  the infrastructure that counts crates downloads.
  It is deployed in the AWS accounts `crates-io-prod` and `crates-io-staging`.
- [crates-io](https://github.com/rust-lang/simpleinfra/tree/master/terragrunt/modules/crates-io):
  Terraform module for the crates.io infrastructure.
- [crates-io-heroku-metrics](https://github.com/rust-lang/simpleinfra/tree/master/terraform/crates-io-heroku-metrics):
  Terraform module that deploys the Heroku metrics [collector](https://github.com/rust-lang/crates-io-heroku-metrics)
  used to gather crates.io's metrics.

## Versions

These are the versions we need to keep up-to-date:

- Terraform providers in `simpleinfra` repo.
- Rust dependencies in the
  [Cargo.toml](https://github.com/rust-lang/simpleinfra/blob/master/terragrunt/modules/crates-io/compute-static/Cargo.toml)
  of the fastly CDN (`Compute@Edge`).
- crates-io-heroku-metrics [Dockerfile](https://github.com/rust-lang/crates-io-heroku-metrics/blob/main/Dockerfile):
  - Ubuntu.
  - Vector.
