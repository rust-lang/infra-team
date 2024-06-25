# About Permissions

> Permissions define the type of access a user has to a given resource.
> Typically, permissions give a user the right to read, edit, or delete an
> object.
> (_[Source](https://docs.datadoghq.com/account_management/rbac/permissions/)_)

We use [Datadog] to monitor our infrastructure and collect logs in a central
location. This document outlines how permissions work and how we are granting
them to users and teams.

## Roles and Teams

Permissions on [Datadog] are always assigned to a _role_. What a user can do on
Datadog is thus determined by the roles they have.

_Teams_, on the other hand, are mostly used to either filter data or to make it
easier to find certain resources. The list of dashboards, for example, can be
filtered to only show dashboards that are tagged with the user's teams.

## Permissions for Metrics

Metrics on [Datadog] can be seen and explored by every user. It is not possible
to limit access to particular metrics, e.g. to grant a team access to only the
metrics from their app.

### Custom Metrics

The creation of custom metrics is limited to a few roles, because they are not a
free resource on Datadog. To ensure that we manage our costs responsibly,
creating new custom metrics must be done in collaboration with an administrator
who can monitor the impact that the new metrics have an our monthly billing.

## Permissions for Dashboards

Dashboards can be restricted in a few different ways:

- Access to specific dashboards can be restricted to teams.
- The creation of new dashboards can be limited to certain roles.
- Making dashboards public can be limited to certain roles.

Dashboards are generally available to all users on [Datadog], only the creation
of new dashboards is disabled for some roles. Sharing a dashboard publicly is
disabled for almost all users, except for administrators and staff of the Rust
Foundation.

## Permissions for Logs

We use [Datadog] as a centralized logging platform. Access to logs is restricted
to the teams that need to work with the specific logs. For example, only the
`crates.io` team can see the logs from the Heroku app and the CDNs for
`static.crates.io`.

## Resources

- [Datadog's documentation on RBAC](https://docs.datadoghq.com/account_management/rbac/permissions/)

[datadog]: ./README.md
