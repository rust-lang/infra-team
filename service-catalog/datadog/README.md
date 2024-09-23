# Datadog

[Datadog] is a platform for monitoring and observability. It has integrations to
gather metrics from most cloud providers, and offers additional features like
application performance monitoring or a logging backend.

We use the following features:

- Host-level metrics using the [Datadog Agent](https://docs.datadoghq.com/agent/)
- Platform-level metrics from AWS and Fastly
- Log pipelines for logs from our CDNs, applications, and servers

## Team members

User accounts on Datadog are managed in [terraform/team-members-datadog].

## Integrations

We use the following integrations:

- [aws](https://github.com/rust-lang/simpleinfra/tree/master/terragrunt/modules/datadog-aws)
- [fastly](https://github.com/rust-lang/simpleinfra/tree/master/terragrunt/modules/datadog-fastly)

## Explanations

- [About Permissions](./about-permissions.md)

[datadog]: https://www.datadoghq.com/
[terraform/team-members-datadog]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/team-members-datadog
