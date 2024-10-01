# Docs.rs

Docs.rs is a service that hosts documentation of crates.

The docs.rs codebase is available at [rust-lang/docs.rs] and
it's maintained by the [docs.rs team](https://www.rust-lang.org/governance/teams/dev-tools#team-docs-rs).

The infrastructure is managed via the [terraform/docs-rs] module.

The EC2 `docsrs.infra.rust-lang.org` is served by Cloudfront CDN at `docs.rs`.

Docs.rs also uses the `rust-docs-rs` S3 bucket which is served
through the docs.rs webserver itself, and additionally through the Cloudfront CDN `static.docs.rs`.
Docs.rs rewrites the doc pages before serving them.

[rust-lang/docs.rs]: https://github.com/rust-lang/docs.rs
[terraform/docs-rs]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/docs-rs
