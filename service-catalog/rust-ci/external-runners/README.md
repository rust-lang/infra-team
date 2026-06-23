# External CI runners

The Rust Project mainly runs Continuous Integration (CI) in GitHub Actions.
When GitHub Actions does not natively support a target, such as RISC-V, we use
one of these options:

- Hardware emulation, for example with QEMU.
- Cross-compilation without running tests on the target, when compile-only
  coverage is enough.
- External CI runners that run jobs on real hardware.

This document explains:

- The requirements external runners must meet to be used in the `rust-lang`
  GitHub organization.
- The process for introducing external CI for Rust targets, including how
  experiments move into `rust-lang/rust`.
- How to configure external runners.

If you want to check what runners are available or you are only interested in
_using external runners_ instead of _configuring them_, read the
[forge](https://forge.rust-lang.org/infra/docs/external-ci-runners.html)
documentation instead of this document.

## GitHub integration

The Infrastructure team accepts external runners in Rust Project CI only if
they integrate with GitHub
[self-hosted runners](https://docs.github.com/en/actions/concepts/runners/self-hosted-runners).

There are two supported integration paths.

### GitHub App (preferred)

A GitHub App can provision self-hosted runners when workflows request them.
We prefer this method because it can create runners on demand.
I.e. runners are ephemeral and are destroyed after the job finishes — no state
persists between jobs.

An example is the
[RISE RISC-V Runners](https://riscv-runners.riseproject.dev/) GitHub App.

Since `rust-lang` is a GitHub organization, the GitHub App only needs write
permissions for _organization self hosted runners_.
GitHub Apps that require more permissions (such as `admin`):

- Cannot be used in `rust-lang/rust`.
- Can be used in other repositories only if the Infrastructure team and the
  repository maintainers agree.

> [!NOTE]
> This is the only integration allowed for the repository `rust-lang/rust`.

### Manual configuration

Self-hosted runners can also be configured manually, although this does not scale as well as a GitHub App.

Each self-hosted runner can run only one job at a time, so the number of
configured runners limits the maximum number of parallel jobs.
For `rust-lang/rust`, configure at least five self-hosted runners so there is enough
capacity for `auto` builds and contributors' try jobs.

With GitHub Apps, we don't have to worry about how many runners to configure
because the GitHub App provisions runners on demand.

Manual setup steps:

1. The Infrastructure admin opens the GitHub organization settings and selects `Actions` ->
   `Runner groups`.
2. _(Skip this step if you are using an existing runner group)_ Click `New runner group`. Then:
   - Give the group a name.
   - Check `Allow public repositories`.
   - Select the repositories that need the runners (don't allow all repositories).
3. Click on the runner group and then on `New runner` -> `New self-hosted runner`.
   The architecture choice and the OS do not
   matter for generating the token.
   GitHub generates a token that can be used to configure the runner. Note:
   - The token expires after one hour.
   - If you click the `New self-hosted runner` button again, the previous token
     is revoked and a new token is generated.
   - The same token can be used to provision multiple runners (no need to click
     `New self-hosted runner` for every runner).
4. The Infrastructure admin copies the token shown after `--token` and sends it
   securely, for example with a password manager link.
5. The runner provider configures the self-hosted runner.
6. The Infrastructure admin verifies that the self-hosted runner was created.
7. The Infrastructure admin communicates the label that Rust Project members
   can use in the workflow `runs-on` field.

## Repository mandatory requirements

Here are the requirements for repositories in the `rust-lang` GitHub
organization to use external runners in their CI workflows.

### Only tests are allowed

External runners can only be used by workflows that run tests. They must not
publish release artifacts or use write-scoped credentials.

The Infrastructure team (`@T-infra`) does not allow distributing artifacts from
external runners for security reasons — if an external runner is compromised, it
must not be able to publish compromised artifacts.

> [!NOTE]
> If you can provide strong [security guarantees](#security-requirements-optional),
> the Infrastructure team is open to discussing this in the `#t-infra` channel.

### Only secure workflows are allowed

Self-hosted runners are a risk for both the Rust Project and the runner
provider because they run untrusted code from pull requests.

We require workflows that use external runners to pass
[zizmor](https://zizmor.sh/) in CI, so that:

- The workflows respect high security standards.
- The workflows are easier to review, ensuring they don't use write-scoped permissions.

## Runner mandatory requirements

These are the requirements that external CI runners must meet to be used in the
`rust-lang` GitHub organization.

If some external runners that were accepted in the past fail to meet these requirements
at some point, the infra team will discuss whether to decommission them or not.

The `Repository` column indicates which repositories the requirement applies to.
For example, a requirement that applies to `rust-lang/rust` must be met for
external runners used in `rust-lang/rust`, but not necessarily for external
runners used in other repositories.

| Requirement                    | Acceptance Criteria                                                                                                                                                                    | Repository       |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- |
| **Operational responsibility** | The external runner providers manage the hardware, OS, runner setup, and software updates. The Rust Infra Team is **not** responsible for operational maintenance of external runners. | `rust-lang/*`    |
| **Runner maintenance**         | The runner is provided and maintained by a team of at least two people.                                                                                                                | `rust-lang/rust` |

## Runner optional requirements

These requirements apply to higher-trust external CI runners. They are optional
for now because external runners can only run test jobs without write-scoped
credentials.

### Operational requirements (optional)

| Requirement                      | Acceptance Criteria                                                                                    |
| -------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Technical contacts**           | At least **two** named technical contacts for escalation, reachable via Zulip.                         |
| **Uptime SLA**                   | 99% monthly uptime (< 8h downtime/month). Downtime measured against the agreed service window.         |
| **Incident response SLA**        | The runner provider provides timelines for acknowledgment and mitigation.                              |
| **Security incident procedures** | Documented process for physical shutdown and forensics access, such as drive removal or image capture. |

### Security requirements (optional)

| Requirement             | Acceptance Criteria                                                                                                                                     |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Access controls**     | Auditable access logs for the build environment, provided when requested by the Infrastructure team.                                                    |
| **Ephemeral builds**    | Build jobs run in environments that are re-imaged or destroyed after each job (mandatory for `rust-lang/rust`).                                         |
| **Endpoint protection** | Active monitoring for malware, CVEs, and intrusion attempts, with quick remediation and alerts sent to the Infrastructure team and the Rust Foundation. |

## Adoption process

If you want to provide external CI runners to the Rust Project, you can follow the
following process:

1. Read this document carefully
2. Ask in the [t-infra](https://rust-lang.zulipchat.com/#narrow/channel/242791-t-infra)
   Zulip channel if someone is interested in adopting the runners.
   Usually, we don't adopt new runners directly in the repository `rust-lang/rust` at first.
   Instead, we adopt new runners in repositories such as
   [`rust-lang/compiler-builtins`](https://github.com/rust-lang/compiler-builtins)
   and [`rust-lang/libc`](https://github.com/rust-lang/libc) to validate the runner
   behavior and reliability.
3. Once you agreed with the team, collaborate with an infra admin to configure the new runners
   (the [GitHub App](#github-app-preferred) method is preferred).
   The infra admin will restrict the runner access to the agreed repositories.
4. Open a PR in the repository to add the runners in the CI.
   Make sure about the following:
   - The new CI job doesn't make the overall CI slower
     (i.e. the new job isn't slower than the existing ones). If that's the case,
     discuss with the team if this is acceptable.
   - The CI workflow file passes [zizmor](https://zizmor.sh/) in CI and doesn't
     use write-scoped credentials.
     The infrastructure team is available to help if needed.
5. Document the new runners in the
   [forge](https://forge.rust-lang.org/infra/docs/external-ci-runners.html).

> [!NOTE]
> Since it's hard to debug issues on unconventional architectures,
> we prefer not adding a runner to `rust-lang/rust` unless there's a proposal for
> raising the target to Tier 1. Until a target is preparing for Tier 1, there's not much
> reason for native hardware: tests are generally not going to be run and so
> cross-compiled builds are sufficient.
>
> See also [target policy](https://doc.rust-lang.org/nightly/rustc/target-tier-policy.html).
