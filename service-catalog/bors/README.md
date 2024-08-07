# Bors

The bors [module](https://github.com/rust-lang/simpleinfra/tree/master/terragrunt/modules/bors)
contains the infrastructure of [bors](https://github.com/rust-lang/bors), the Rust rewrite of
[homu](https://github.com/rust-lang/homu).

`Homu` is deployed in the legacy account, while `bors` is deployed in the
`bors-staging` and `bors-prod` account, depending on the environment.

Bors is deployed as a [Fargate](https://aws.amazon.com/fargate/) service
([ECS](https://aws.amazon.com/ecs/)) in the `us-east-2` region.
Bors uses an RDS PostgreSQL database.

To deploy a new version, the
[deployment](https://github.com/rust-lang/bors/blob/main/.github/workflows/deploy.yml)
pipeline:

1. Pushes the Docker image to the [ECR](https://aws.amazon.com/ecr/) repository.
2. Updates the ECS service, forcing a new deployment.

## Versions

These are the versions we need to keep up-to-date:

- Operating system: Ubuntu. Version specified in the
  [Dockerfile]
- Rust toolchain: specified in the [Dockerfile]
- Rust dependencies: specified in the
  [Cargo.toml](https://github.com/rust-lang/bors/blob/main/Cargo.toml)
- PostgreSQL: version specified in
  [Terraform](https://github.com/rust-lang/simpleinfra/blob/master/terragrunt/modules/bors/main.tf)

No automation is in place as of August 2024.

[Dockerfile]: https://github.com/rust-lang/bors/blob/main/Dockerfile
