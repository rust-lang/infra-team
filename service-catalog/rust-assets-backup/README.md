# Rust assets backup

> [!NOTE]
> GCP backups are still under development.
> Not everything documented here is implemented.

## Summary

In GCP (Google Cloud Platform) we keep offsite backups for both Rust releases and crates
to protect us against security threats that could involve losing crates or releases.
These threats were identified in a [threat model] for the project's infrastructure, created by the Rust Foundation's security engineer Walter.

## Motivation

While we have multiple measures in place to prevent accidental deletion of Rust releases or crates in AWS,
e.g. bucket replication to a different region and restricted access, our current setup does not sufficiently protect us against a few threats:

1. _AWS Account compromise_. The [threat model] highlights the risk of an AWS account compromise.
   If a malicious actor was able to gain administrator access to the AWS account of one of the [infra-admins],
   they could bypass a lot of safe guards and delete data.
2. _AWS Account deletion_. AWS could accidentally delete our account, resulting in the possible deletion of data and backups.
   Something similar happened to a customer on [GCP](https://arstechnica.com/gadgets/2024/05/google-cloud-accidentally-nukes-customer-account-causes-two-weeks-of-downtime/) in 2024.

- To mitigate threat 1, the new backup needs to have separate admin access.
- To mitigate threat 2, the new backup needs to be in a separate cloud environment.

## Implementation overview

These new backups are hosted in a dedicated GCP account and have totally separate access controls compared to AWS.
Specifically, none of the current `infra-admins` have admin access to this separate environment to protect against an account compromise.
This GCP account is not used for anything else (just for backups).

The backups are automatically copied daily by GCP.

### Access 👤

We limit admin access to the GCP backups to two members of the Rust Foundation for the following reasons:

- _ensure a strong separation of access_: as explained in the first [motivation](#Motivation), the GCP admins should be different from the AWS admins.
  This means we can't give admin access to any of the `infra-admins`.
- _accountability_: The Rust Foundation employees have signed an employment contract and can be legally liable for malicious actions.

People with admin access to the GCP account:

- Joel Marcey (Director of Technology @ Rust Foundation)
- Walter Pearce (Security Engineer @ Rust Foundation)

People with read-only access to the GCP project:

- `infra-admins`

The admin access of the GCP account is bound to the `@rustfoundation.org` Google Workspace account.
This means that if an employee leaves the Rust Foundation, they lose access to the GCP account.
In this case we need to add a new admin to the GCP account.

> [!NOTE]
> The `infra-admins` team can have admin access to the GCP staging project if needed.

### In case of emergency 🧯

- In case our data in AWS is deleted, the `infra-admin` team can restore it by:
  - copying the data from GCP to AWS using the GCP read-only access.
  - restoring the `crates-io-index` bucket from the `db-dump` stored in the `crates-io` bucket. Use [this](https://github.com/rust-lang/crates.io/blob/e0bb0049daa12f5362def463b04febd6c036d315/src/worker/jobs/git.rs#L19-L129) code.
- If the GCP synchronization mechanism breaks, the Infrastructure team can raise a PR to fix the Terraform configuration and a GCP admin can apply it.

### New threat model 🦹

To delete our data, an attacker would need to compromise both:

- one AWS admin account (an `infra-admin`)
- one GCP admin account (Joel or Walter)

This improves our security posture because compromising two accounts is harder than compromising one.

The accidental account deletion is not a threat anymore because if either AWS or GCP delete our account, we can restore the data from the other provider.

## Implementation details

The account where we store the backup is called `rust-backup`. It contains two GCP projects: `backup-prod` and `backup-staging`.
Here we have one Google [Object Storage](https://cloud.google.com/storage?hl=en) in the `europe-west1` (Belgium) region for the following AWS S3 buckets:

- `crates-io`. CloudFront URL: `cloudfront-static.crates.io`. It contains the crates published by the Rust community.
- `static-rust-lang-org`. CloudFront Url: `cloudfront-static.rust-lang.org`. Among other things, it contains the Rust releases.

For the objects:

- The [storage class](https://cloud.google.com/storage/docs/storage-classes) is set to "archive" for all buckets.
  This is the cheapest class for infrequent access.
- [object-versioning](https://cloud.google.com/storage/docs/object-versioning) and [soft-delete](https://cloud.google.com/storage/docs/soft-delete) are enabled,
  so that we can recover updates and deletes.

We use [Storage Transfer](https://cloud.google.com/storage-transfer/docs/overview) to automatically transfer the content of the s3 bucket into the Google Object Storage.
This is a service managed by Google. We'll use it to download the S3 buckets from CloudFront to perform a daily incremental transfer. The transfers only move files that are new, updated, or deleted since the last transfer, minimizing the amount of data that needs to be transferred.

## Explanations

- [FAQ](./faq.md)

## How-to Guides

- [Maintenance](./maintenance.md)

[infra-admins]: https://github.com/rust-lang/team/blob/master/teams/infra-admins.toml
[threat model]: https://docs.google.com/document/d/10Qlf8lk7VbpWhA0wHqJj4syYuUVr8rkGVM-k2qkb0QE
