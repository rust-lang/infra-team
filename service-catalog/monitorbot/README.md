# Monitorbot

Bot that monitors various APIs and services that the infrastructure team uses.
For example, it's used to check if GitHub tokens are getting close to their rate limits.

[terraform/monitorbot] deploys the monitorbot as an ECS app.

## Versions

These are the versions we need to keep up-to-date:

- Ubuntu version of the [Dockerfile]
- Dependencies of [Cargo.toml]
- GitHub Actions versions in [workflows]

[Dockerfile]: https://github.com/rust-lang/monitorbot/blob/master/Dockerfile
[Cargo.toml]: https://github.com/rust-lang/monitorbot/blob/master/Cargo.toml
[workflows]: https://github.com/rust-lang/monitorbot/blob/master/.github/workflows/main.yml
[terraform/monitorbot]: https://github.com/rust-lang/simpleinfra/blob/master/terraform/monitorbot/
