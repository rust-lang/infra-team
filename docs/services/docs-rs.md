# docs.rs

* Source code: [rust-lang/docs.rs][repo]
* Hosted on: `docsrs.infra.rust-lang.org` (behind the bastion)
* Maintainers: [onur][onur], [QuietMisdreavus][QuietMisdreavus]

[repo]: https://github.com/rust-lang/docs.rs
[onur]: https://github.com/onur
[QuietMisdreavus]: https://github.com/QuietMisdreavus

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

### Cleaning up the target directory

Cleaning up the target directory is the best way to reclaim disk space on the
instance, in the event it was full. To do that you need to first open a shell
inside the container:

```
$ sudo lxc-attach -n docs-rs-container bash
```

...and then inside the shell you can remove all the target directory's contents:

```
$ rm -rf /home/cratesfyi/cratesfyi/*
```

If docs.rs went down due to the full disks you'll also want to first restart
PostgreSQL and then docs.rs:

```
$ sudo systemctl restart postgresql
$ sudo systemctl restart docs.rs
```

### Kill stuck builds

Sometimes a build could get stuck, clogging up the queue. You first need to
remove the crate that failed from the queue (see [the related section of this
doc](#temporarily-remove-a-crate-from-the-queue)), and then you can execute
this command to kill all stuck processes:

```
$ unstuck-build
```
