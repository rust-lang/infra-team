# docs.rs

* Source code: [rust-lang/docs.rs][repo]
* Hosted on: `docsrs.infra.rust-lang.org` (behind the bastion -- [how to connect][bastion-connect])
* Maintainers: [onur][onur], [QuietMisdreavus][QuietMisdreavus]
* [Instance metrics][grafana-instance] (only available to infra team members).
* [Application metrics][grafana-app] (only available to infra team members).

## Common maintenance procedures

### Temporarily remove a crate from the queue

It might happen that a crate fails to build repeatedly due to a docs.rs bug,
clogging up the queue and preventing other crates to build. In this case it's
possible to temporarily remove the crate from the queue until the docs.rs's bug
is fixed. To do that, log into the machine and open a PostgreSQL shell with:

```
$ psql
```

Then you can run this SQL query to remove the crate:

```
UPDATE queue SET attempt = 100 WHERE name = '<CRATE_NAME>';
```

To add the crate back in the queue you can run in the PostgreSQL shell this
query:

```
UPDATE queue SET attempt = 0 WHERE name = '<CRATE_NAME>';
```

[repo]: https://github.com/rust-lang/docs.rs
[grafana-instance]: https://grafana.rust-lang.org/d/rpXrFfKWz/instance-metrics?orgId=1&var-instance=docsrs.infra.rust-lang.org:9100
[grafana-app]: https://grafana.rust-lang.org/d/-wWFg2cZz/docs-rs?orgId=1
[bastion-connect]: https://github.com/rust-lang/infra-team/blob/master/docs/hosts/bastion.md#logging-into-servers-through-the-bastion
[onur]: https://github.com/onur
[QuietMisdreavus]: https://github.com/QuietMisdreavus
