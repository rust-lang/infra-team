# Discord moderation bot

[Discord-mods-bot](https://github.com/rust-lang/discord-mods-bot)
is a bot used by the moderation team to manage the Rust Discord server.

The bot is managed in the
[terraform/discord-mods-bot](https://github.com/rust-lang/simpleinfra/tree/master/terraform/discord-mods-bot)
module and uses the `discord-mods-bot` database of
[terraform/rds-databases](https://github.com/rust-lang/simpleinfra/tree/master/terraform/rds-databases)

## Versions

Here are the dependencies we need to keep up-to-date:

`simpleinfra` repo:

- terraform providers
- `aws_ecs_service` platform version

`discord-mods-bot` repo:

- Ubuntu version specified in the
  [Dockerfile](https://github.com/rust-lang/discord-mods-bot/blob/master/Dockerfile)
- Rust dependencies: specified in the
  [Cargo.toml](https://github.com/rust-lang/discord-mods-bot/blob/master/Cargo.toml)
- Postgres version
- GitHub Actions: specified in the
  [workflows](https://github.com/rust-lang/discord-mods-bot/tree/master/.github/workflows)
  directory
