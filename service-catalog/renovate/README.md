# Renovate

[Renovate](https://docs.renovatebot.com/) is our preferred way to
handle dependency updates. It is more configurable than Dependabot and can be
used to keep dependencies such as crates, GitHub Actions and Docker base images
up-to-date.

See the existing configuration files in the Rust organization for examples:
[GitHub code search for `renovate.json` paths](https://github.com/search?q=org%3Arust-lang+path%3Arenovate.json&type=code).

## Renovate app vs. `forking-renovate`

There are two GitHub Apps that can run Renovate:

- The [`renovate` GitHub App](https://github.com/apps/renovate) creates
  update branches directly in the target repository. That requires write access
  to repository contents, but it also supports the full feature set, including
  automerge.
- The [`forking-renovate` GitHub App](https://github.com/apps/forking-renovate)
  creates branches in its own fork and opens PRs back to the target
  repository. That reduces the permissions it needs on the target repository,
  but it only works for public repositories and does not support
  automerge.

## After adding config files

After adding Renovate config files to a repository, infra admins need to:

1. Go to [developer.mend.io/github/rust-lang](https://developer.mend.io/github/rust-lang).
2. Check that the repository is enabled.
3. Check that Renovate has run at least once.
   If not, trigger a run by clicking "Run" on the repository's page.
4. Check that the dependency dashboard issue was created, so maintainers can
   trigger PRs in their repositories by interacting with that issue.
