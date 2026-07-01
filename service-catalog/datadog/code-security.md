# Code Security

[Datadog Code Security] scans repositories for security issues in first-party
code and open source dependencies.
Rust Project members can opt in repositories
they maintain so Datadog can analyze them and they can analyze the DataDog
findings in the
[DataDog Code Security UI](https://app.datadoghq.com/security/code-security/summary).

## Opting in

To enable Code Security for a Rust Project repository:

1. Make sure the Datadog app bot has access to the repository through the
   repository configuration in [rust-lang/team]:

   ```toml
   bots = ["datadog"]
   ```

2. Ask an infra admin in the [t-infra] Zulip channel to enable Code Security for
   the repository.

[datadog code security]: https://docs.datadoghq.com/security/code_security/
[rust-lang/team]: https://github.com/rust-lang/team
[t-infra]: https://rust-lang.zulipchat.com/#narrow/channel/242791-t-infra
