# Renovate

The user-facing documentation of Renovate is documented in the
[Rust Forge](https://forge.rust-lang.org/infra/docs/renovate.html).

## Troubleshooting

If Renovate doesn't work on a repository, infra admins can:

1. Go to [developer.mend.io/github/rust-lang](https://developer.mend.io/github/rust-lang).
2. Check that the repository is enabled.
3. Check that Renovate has run at least once.
   If not, trigger a run by clicking "Run" on the repository's page.
4. Check that the dependency dashboard issue was created, so maintainers can
   trigger PRs in their repositories by interacting with that issue.
