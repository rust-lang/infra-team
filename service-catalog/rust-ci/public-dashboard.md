# Public dashboard

Datadog can only be accessed by certain Rust teams.
However certain data are useful to everyone, so the infra team created a
[public dashboard](https://p.datadoghq.com/sb/3a172e20-e9e1-11ed-80e3-da7ad0900002-b5f7bb7e08b664a06b08527da85f7e30)
to monitor the Rust CI.

This dashboard is just a clone of the Datadog CI
[dashboard](https://docs.datadoghq.com/continuous_integration/?site=us).

These are some useful panels from the dashboard:

- Pipeline duration: check how long the auto builds takes to run.
- Top slowest jobs: check which jobs are taking the longest to run.
- Change in median job duration: check what jobs are slowest than before. Useful
  to detect regressions.
- Top failed jobs: check which jobs are failing the most.
