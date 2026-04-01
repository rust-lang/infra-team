# Service Catalog

The service catalog lists the infrastructure, services, and apps that the
maintains. It is a work-in-progress and not exhaustive. It is our intention to
improve it as we go.

The documentation in the service catalog is inspired by the [Diataxis]
framework, which proposes a systematic approach to categorize into tutorials,
how-to guides, explanations, and reference documentation.

## For Contributors

- [bastion](./bastion/README.md)
- [bors](./bors/README.md)
- [ci-mirrors](./ci-mirrors/README.md)
- [crater](./crater/README.md)
- [crates-io](./crates-io/README.md)
- [crates-io-auth-action](./crates-io-auth-action/README.md)
- [dev-desktops](./dev-desktops/README.md)
- [discord-mods-bot](./discord-mods-bot/README.md) _(decommissioned)_
- [dns](./dns/README.md)
- [docs-rs](./docs-rs/README.md)
- [IBM runners](./ibm-runners/README.md)
- [infra-smoke-tests](./infra-smoke-tests/README.md)
- [monitorbot](./monitorbot/README.md)
- [playground](./playground/README.md)
- [releases](./releases/README.md)
- [rfcbot](./rfcbot/README.md)
- [rust-ci](./rust-ci/README.md)
- [rust-forge](./rust-forge/README.md)
- [rust-log-analyzer](./rust-log-analyzer/README.md)
- [rustc-perf](./rustc-perf/README.md)
- [rustup](./rustup/README.md)
- [sync-team](./sync-team/README.md)
- [team-member-access](./team-member-access/README.md)
- [triagebot](./triagebot/README.md)
- [users.rust-lang.org](./users-rust-lang-org/README.md)

### Tracking websites

- The state of tools included with Rust are tracked on the
  [toolstate page](https://rust-lang-nursery.github.io/rust-toolstate/).
  When each PR is merged via CI, the status of each tool is recorded in a JSON
  file and stored in the
  [toolstate repo](https://github.com/rust-lang-nursery/rust-toolstate).
  For further information, see the
  [toolstate system documentation][toolstate-docs].
- The [rustup components history][rustup-components-history] tracks
  the status of every rustup component for every platform over time. See the
  repository for more information.

[rustup-components-history]: https://rust-lang.github.io/rustup-components-history/
[toolstate-docs]: https://forge.rust-lang.org/infra/toolstate.html

## External Services

- [Datadog](./datadog/README.md)
- [Fastly](./fastly/README.md)
- [Renovate](./renovate/README.md)

[diataxis]: https://diataxis.fr/
