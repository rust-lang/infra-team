# IBM runners

The Rust Project uses GitHub Actions runners provided by IBM in
[actionspz](https://github.com/IBM/actionspz).

To use the runners, we installed the GitHub App
[power-z-gha-runner](https://github.com/apps/power-z-gha-runner/) from the
[rust-lang-owner](https://github.com/rust-lang-owner) user account.

As of August 2025, the app is installed in the following repositories:

- [libc](https://github.com/rust-lang/libc)
- [stdarch](https://github.com/rust-lang/stdarch)
- [compiler-builtins](https://github.com/rust-lang/compiler-builtins)

The runners were requested in [this](https://github.com/IBM/actionspz/issues/24) and the linked GitHub issues.

These repositories can use the runners `ubuntu-24.04-ppc64le` and `ubuntu-24.04-s390x`.

We haven't installed the GitHub App in the `rust-lang/rust` repository
because the app requires admin permission to run.
See [this](https://github.com/IBM/actionspz/issues/20) GitHub issue for more info.
