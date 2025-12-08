# Triagebot

[`triagebot`][triagebot] is a bot that is used for various actions related to issue
triaging, pull request assignments, Zulip meeting management and many more.
Users interact with it either through GitHub comments or Zulip commands.

It is deployed using [terraform/triagebot]. The bot is maintained
by the [triagebot][triagebot-team] team.

It uses an ECR repository to store the Docker image and an ECS service to run
it.

It also uses the `shared` db of [rds-databases].

For more information, see the [triagebot docs].

## Versions

These are the versions we need to keep up-to-date:

- Node and Ubuntu version specified in the [Dockerfile]
- Dependencies of [Cargo.lock]
- Postgres version of the `shared` database
- GitHub Actions versions in [workflows]

[triagebot]: https://github.com/rust-lang/triagebot
[terraform/triagebot]: https://github.com/rust-lang/simpleinfra/blob/master/terraform/shared/services/triagebot/main.tf
[triagebot-team]: https://github.com/orgs/rust-lang/teams/triagebot
[rds-databases]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/rds-databases
[Dockerfile]: https://github.com/rust-lang/triagebot/blob/master/Dockerfile
[Cargo.lock]: https://github.com/rust-lang/triagebot/blob/master/Cargo.lock
[workflows]: https://github.com/rust-lang/triagebot/tree/master/.github/workflows
[triagebot docs]: https://github.com/rust-lang/triagebot
