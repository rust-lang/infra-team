# Policy on broken nightlies

Sometimes the nightlies released automatically by our CI ends up being broken
for some people or even everyone. This policy defines what the infra team
response will be in those cases.

## Which nightly will be rolled back

* A nightly can be rolled back if it contains destructive code, for
  example if the included compiler deletes all the users files.

* A nightly can be rolled back if an infra problem caused it to be broken for a
  big percentage of users on any Tier 1 platform. Issues affecting only lower
  tier platforms are not worthy of a roll back, since we don't guarantee working
  builds for those platforms anyway.

* A nightly will **not** be rolled back if it's broken by a critical compiler
  bug: those bugs are supposed to be caught by CI, and nightly is supposed to
  have compiler regressions.

There are no exceptions the policy, even if big projects are broken because of
this.

## What are we going to fix

When the infra team decides a nightly will be rolled back, we will only fix
installing the nightly with rustup:

```
$ rustup toolchain install nightly
```

Other things like the documentation or the artifacts you can manually download
will not be rolled back.
