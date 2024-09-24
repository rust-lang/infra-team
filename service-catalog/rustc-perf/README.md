# Rustc perf

[Rustc perf] is a website that tracks the performance of
`rustc` over time.

It is deployed using [terraform/rustc-perf].
The app is in the [rustc-perf]
repository and it's maintained by the [wg-compiler-performance] team.

It uses an ECR repository to store the Docker image and an ECS service to run it.

It also uses the `shared` db of [rds-databases] and the `rustc-perf` S3 bucket.

The collector (the machine that runs the benchmarks) is a dedicated physical server running at [Hetzner].

[Rustc perf]: https://perf.rust-lang.org/
[rustc-perf]: https://github.com/rust-lang/rustc-perf
[terraform/rustc-perf]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/rustc-perf
[wg-compiler-performance]: https://github.com/orgs/rust-lang/teams/wg-compiler-performance
[rds-databases]: https://github.com/rust-lang/simpleinfra/tree/master/terraform/rds-databases
[Hetzner]: https://www.hetzner.com/
