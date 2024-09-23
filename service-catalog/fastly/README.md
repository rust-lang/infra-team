# Fastly

[Fastly] is a [Content Delivery Network] (CDN) that caches content and makes it
available closer to the user.

## Fastly Exporter

[terraform/fastly-exporter] deploys the fastly-exporter, which makes the [Fastly Real-time analytics](https://www.fastly.com/documentation/reference/api/metrics-stats/realtime/) data available to Prometheus.

## Team members

User accounts on Fastly are managed in [terraform/team-members-fastly].

### Versions

These are the versions we need to keep up-to-date:

- Docker version of `ghcr.io/fastly/fastly-exporter`
- Terraform providers

## How-to Guides

- [How to test cache invalidations](./how-to-test-cache-invalidations.md)

[content delivery network]: https://en.wikipedia.org/wiki/Content_delivery_network
[fastly]: https://www.fastly.com/
[terraform/fastly-exporter]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/fastly-exporter
[terraform/team-members-fastly]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/team-members-fastly
