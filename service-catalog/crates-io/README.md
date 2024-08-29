# Crates.io

Crates.io is the Rust community's package registry.
The crates.io
[team](https://www.rust-lang.org/governance/teams/dev-tools#team-crates-io)
develops [crates-io](https://github.com/rust-lang/crates.io)
and the Infra team helps with the infrastructure.

The crates.io app is deployed in Heroku, you can learn more about it in the
crates-io [docs](https://github.com/rust-lang/crates.io/blob/main/docs/ARCHITECTURE.md).

Here are the details of the crates.io infrastructure managed by the infra team:

- [crates-io-logs](https://github.com/rust-lang/simpleinfra/tree/master/terragrunt/modules/crates-io-logs):
  the infrastructure that counts crates downloads.
  It is deployed in the AWS accounts `crates-io-prod` and `crates-io-staging`.

## Versions

These are the versions we need to keep up-to-date:

- Terraform providers in `simpleinfra` repo.
