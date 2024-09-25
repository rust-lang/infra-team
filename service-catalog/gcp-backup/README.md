# GCP backup

## Summary

In GCP (Google Cloud Platform) we keep offsite backups for both Rust releases and crates
to protect us against security threats that could involve losing crates or releases.
These threats were identified in a [threat model] for the project's infrastructure, created by the Rust Foundation's security engineer Walter.

## Motivation

While we have multiple measures in place to prevent accidental deletion of Rust releases or crates in AWS,
e.g. bucket replication to a different region and restricted access, our current setup does not sufficiently protect us against a few threats:

1. _AWS Account compromise_. The [threat model] for Rust's infrastructure, created by the Rust Foundation's security engineer, highlights the risk of an AWS account compromise.
   If a malicious actor was able to gain administrator access to the AWS account of one of the [infra-admins],
   they could bypass a lot of safe guards and delete data.
2. _AWS Account deletion_. AWS could accidentally delete our account, resulting in the possible deletion of data and backups.
   For example, something similar happened at [Google](https://arstechnica.com/gadgets/2024/05/google-cloud-accidentally-nukes-customer-account-causes-two-weeks-of-downtime/) recently.

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
- _accountability_: the Rust Foundation employees are accountable for their actions, which means they can't delete the backups intentionally without breaking the law.

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
- If the GCP synchronization mechanism breaks, the Infra team can raise a PR to fix the terraform configuration and a GCP admin can apply it.

### New threat model 🦹

To delete our data, an attacker would need to compromise both:

- one AWS admin account (an `infra-admin`)
- one GCP admin account (Joel or Walter)

This improves our security posture because compromising two accounts is harder than compromising one.

The accidental account deletion is not a threat anymore because if either AWS or GCP delete our account, we can restore the data from the other provider.

## Implementation details

The account where we store the backup is called `rust-backup`. It contains two GCP projects: `backup-prod` and `backup-staging`.
Here we have one Google [Object Storage](https://cloud.google.com/storage?hl=en) in the `europe-west1` (Belgium) region for the following AWS S3 buckets:

- `crates-io`. Cloudfront URL: `cloudfront-static.crates.io`. It contains the crates published by the Rust community.
- `static-rust-lang-org`. Cloudfront Url: `cloudfront-static.rust-lang.org`. Among other things, it contains the Rust releases.

For the objects:

- Set the [storage class](https://cloud.google.com/storage/docs/storage-classes) to "archive" for all buckets.
  This is the cheapest class for infrequent access.
- Enable [object-versioning](https://cloud.google.com/storage/docs/object-versioning) and [soft-delete](https://cloud.google.com/storage/docs/soft-delete),
  so that we can recover updates and deletes so that we can recover updates and deletes.

We use [Storage Transfer](https://cloud.google.com/storage-transfer/docs/overview) to automatically transfer the content of the s3 bucket into the Google Object Storage.
This is a service managed by Google. We'll use it to download the S3 buckets from cloudfront to perform a daily incremental transfer. The transfers only move files that are new, updated, or deleted since the last transfer, minimizing the amount of data that needs to be transferred.

### Monitoring 🕵️

To check that the backups are working:

- Ensure the number of files and the size of the GCP buckets is the same as the respective AWS buckets by looking at the metrics
- Ensure that only the authorized people have access to the account

You can also run the following test:

- Upload a file in an AWS S3 bucket and check that it appears in GCP.
- Edit the file in AWS and check that you can recover the previous version from GCP.
- Delete the in AWS and check that you can recover all previous versions from GCP.

In the future, we might want to create alerts in:

- _Datadog_: to monitor if the transfer job fails.
- _Wiz_: to monitor if the access control changes.

### Backup maintenance 🧹

If a crate version is deleted from the crates-io bucket (e.g. for GDPR reasons), an admin needs to delete it from the GCP bucket as well.
Even though the delete will propagate to GCP, the `soft-delete` feature will preserve the data, so we need to delete it manually.

### FAQ 🤔

#### Do we need a multi-region backup for the object storage?

No. [Multi-region](https://cloud.google.com/storage/docs/availability-durability#cross-region-redundancy) only helps if we want to serve this data real-time and we want to have a fallback mechanism if a GCP region fails. We just need this object storage for backup purposes, so we don't need to pay more 👍

#### Why did you choose the `europe-west1` GCP region?

It's far from the `us-west-1` region where the AWS S3 buckets are located. This protects us from geographical disasters.
The con is that the latency of the transfer job is higher when compared to a region in the US.
Also, the cost calculator indicates that this regions has a "Low CO2" and it's among the cheapest regions.

#### Why GCP?

Both the Rust Foundation and the Rust project have a good working relationship with Google, and it is where the Rust Foundation's Security Initiative hosts its infrastructure.
Due to the good collaboration with Google, we expect that we can cover the costs of the backup with credits provided by Google.

[infra-admins]: https://github.com/rust-lang/team/blob/master/teams/infra-admins.toml
[threat model]: https://docs.google.com/document/d/10Qlf8lk7VbpWhA0wHqJj4syYuUVr8rkGVM-k2qkb0QE
