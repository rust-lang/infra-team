# Website

The [Fastly website] shows interesting analytics about our CDN usage.
Most of the analytics are also available in [Datadog], but in Fastly you can see
more info, such as the hardware usage in Fastly (CPU and RAM).

## Side panel

The side panel provides access to various features and settings:

- _Observability_: Some features (such as the [Logs explorer]) are disabled, because
  we stream everything to Datadog.
- _CDN_: Services that runs with [vcl]
- _Compute_: Services that runs with the Fastly's edge [Compute Platform].
- _Account_: Token configuration and billing information.

## Fiddle

[Fastly fiddle] allows to test functions online (similar to the Rust playground).
You can find an example for `static.rust-lang.org` [here](https://fiddle.fastly.dev/fiddle/eb4b0dfb).

[Fastly website]: https://manage.fastly.com/
[Datadog]: ../datadog/README.md
[Logs explorer]: https://manage.fastly.com/observability/logs/explorer
[vcl]: https://www.fastly.com/documentation/guides/full-site-delivery/fastly-vcl/about-fastly-vcl/
[Compute Platform]: https://www.fastly.com/documentation/reference/compute/
[Fastly fiddle]: https://fiddle.fastly.dev/
