# Github Advanced Security

We adopt [Github Advanced Security] features to further enhance `rust-lang` Github organization security.

## Secrets Protection

We enabled [Github Secret Protection] to all non-private, non-forks repositories in `rust-lang`,
with the following configuration:

- validity checks against all available [Secret Scanning partners] by default
- push protection enabled with no bypass privileges by default
- generic passwords **not scanned** with AI by default

Any findings will be available in the Security tab of a repository, like
[rust-lang/crates.io/security], which is visible to maintainers and Github administrators.

[Github Advanced Security]: https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security
[Github Secret Protection]: https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security#github-secret-protection
[Secret Scanning partners]: https://docs.github.com/en/code-security/concepts/secret-security/about-secret-scanning-for-partners
[rust-lang/crates.io/security]: https://github.com/rust-lang/crates.io/security
