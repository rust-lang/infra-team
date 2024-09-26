# Sync team

[sync-team](https://github.com/rust-lang/sync-team) is a CLI tool used to
synchronize the contents of the
[rust-lang/team](https://github.com/rust-lang/team)
repository with the services we use, such as GitHub, and Zulip.

The infastructure lives in
[terraform/team-repo](https://github.com/rust-lang/simpleinfra/tree/master/terraform/team-repo)
and consists of:

- ECR repository for storing the Docker image of the sync-team CLI.
- A lambda function that runs sync-team through CodeBuild.

## Versions

`simpleinfra` repo:

- Terraform providers
- Node.js runtime for the lambda function

`sync-team` repo:

- Rust toolchain: specified in
  [rust-toolchain.toml](https://github.com/rust-lang/sync-team/blob/master/rust-toolchain.toml)
- Rust dependencies: specified in the
  [Cargo.toml](https://github.com/rust-lang/sync-team/blob/master/Cargo.toml)
- Ubuntu version specified in the
  [Dockerfile](https://github.com/rust-lang/sync-team/blob/master/Dockerfile)
- GitHub Actions: specified in the
  [workflows](https://github.com/rust-lang/sync-team/tree/master/.github/workflows)
  directory

No automation is in place as of August 2024.
