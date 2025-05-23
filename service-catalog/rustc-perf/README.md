# Rustc perf

[Rustc perf] is a website that tracks the performance of `rustc` over time.
It hosts various dashboards, charts and comparisons used to analyze
how individual changes (pull requests) to the compiler affect its performance.

It is deployed using [terraform/rustc-perf]. The app is in the [rustc-perf]
repository and is maintained by the [wg-compiler-performance] team.

It uses an ECR repository to store the Docker image and an ECS service to run
it.

It also uses the `shared` db of [rds-databases] and the `rustc-perf` S3 bucket.

The collector (the machine that runs the benchmarks) is a dedicated physical
server running at [Hetzner]. Rustc-perf runs on a bare-metal server because we
need predictable performance. Otherwise, test results might be spoiled by noisy
neighbors in the cloud.

For more information, see the [rustc-perf docs].

## Versions

These are the versions we need to keep up-to-date:

- Node and Ubuntu version specified in the [Dockerfile]
- Dependencies of [Cargo.lock] and [package-lock.json]
- Postgres version of the `shared` database
- GitHub Actions versions in [workflows]
- Operating system of the Hetzner server

[Rustc perf]: https://perf.rust-lang.org/
[rustc-perf]: https://github.com/rust-lang/rustc-perf
[terraform/rustc-perf]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/rustc-perf
[wg-compiler-performance]: https://github.com/orgs/rust-lang/teams/wg-compiler-performance
[rds-databases]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/rds-databases
[Hetzner]: https://www.hetzner.com/
[Dockerfile]: https://github.com/rust-lang/rustc-perf/blob/master/Dockerfile
[Cargo.lock]: https://github.com/rust-lang/rustc-perf/blob/master/Cargo.lock
[package-lock.json]: https://github.com/rust-lang/rustc-perf/blob/master/site/frontend/package-lock.json
[workflows]: https://github.com/rust-lang/rustc-perf/tree/master/.github/workflows
[rustc-perf docs]: https://github.com/rust-lang/rustc-perf/tree/master/docs
