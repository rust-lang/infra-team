# Bors

[Bors](https://github.com/rust-lang/bors) is the Rust rewrite of
[homu](https://github.com/rust-lang/homu).

- The bors infrastructure is managed in the
  [bors](https://github.com/rust-lang/simpleinfra/tree/master/terragrunt/modules/bors)
  terragrunt module and is deployed in the `bors-staging` and `bors-prod`
  account, depending on the environment.
- The `homu` infrastructure is managed in the [terraform/bors](https://github.com/rust-lang/simpleinfra/tree/master/terraform/bors)
  module and is deployed in the legacy account.

Bors is deployed as a [Fargate](https://aws.amazon.com/fargate/) service
([ECS](https://aws.amazon.com/ecs/)) in the `us-east-2` region. Bors uses an RDS
PostgreSQL database.

To deploy a new version, the
[deployment](https://github.com/rust-lang/bors/blob/main/.github/workflows/deploy.yml)
pipeline:

1. Pushes the Docker image to the [ECR](https://aws.amazon.com/ecr/) repository.
2. Updates the ECS service, forcing a new deployment.

## Versions

These are the versions we need to keep up-to-date:

- Ubuntu version specified in the [Dockerfile]
- Rust toolchain: specified in the [Dockerfile]
- Rust dependencies: specified in the
  [Cargo.toml](https://github.com/rust-lang/bors/blob/main/Cargo.toml)
- PostgreSQL: version specified in
  [Terraform](https://github.com/rust-lang/simpleinfra/blob/master/terragrunt/modules/bors/main.tf)
- GitHub Actions: specified in the
  [workflows](https://github.com/rust-lang/bors/tree/main/.github/workflows)
  directory

No automation is in place as of August 2024.

[Dockerfile]: https://github.com/rust-lang/bors/blob/main/Dockerfile
