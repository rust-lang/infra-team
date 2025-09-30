# Implementation details

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
