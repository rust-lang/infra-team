# Crater

`Crater` is a tool to run experiments across parts of the Rust ecosystem.

The `crater` service is managed in the [terraform/crater] module, while the app is in the [rust-lang/crater] repository.

You can find the detailed `crater` docs
[here](https://github.com/rust-lang/crater/tree/master/docs).

## Architecture

The crater project contains the following components:

- `crater`: server that assigns experiments (tasks) to agents
- `agent`: worker that runs experiments and reports back to the server

```mermaid
graph LR
    crater["Crater"]
    agent1["Agent 1"]
    agent2["Agent 2"]

    agent1 --> crater
    agent2 --> crater
```

The agents and crater communicate over HTTP.

The agents communicate to crater their capabilities
(e.g. "windows" or "hard-drive-bigger-than-1TB") and crater assigns experiments
based on these capabilities.

## Bot

Crater can be controlled in the `rust-lang/rust` repo thanks to the GitHub bot
[@craterbot](https://github.com/craterbot). The bot replies to every command in
the comments of issues and pull requests, if the command is in its own line and
is prefixed with the bot's username.

For example, to check if the bot is alive you can write this comment:

```
@craterbot ping
```

And the bot will reply to you.

## Versions

These are the versions we need to keep up-to-date:

- Ubuntu version specified in the [Dockerfile]
- Cargo dependencies: specified in the [Cargo.lock]
- GitHub Actions: specified in the [workflows] directory
- Ubuntu image of the [agent template]

[agent template]: https://github.com/rust-lang/simpleinfra/blob/74bbf479de315fb5c5d6e97832fc3dc9b12e4cab/terraform/crater/agent.tf#L139
[Dockerfile]: https://github.com/rust-lang/crater/blob/master/Dockerfile
[Cargo.lock]: https://github.com/rust-lang/crater/blob/master/Cargo.lock
[workflows]: https://github.com/rust-lang/crater/tree/master/.github/workflows
[rust-lang/crater]: https://github.com/rust-lang/crater
[terraform/crater]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/crater
