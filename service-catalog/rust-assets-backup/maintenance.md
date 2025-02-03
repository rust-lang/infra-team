# Rust Assets Backup: Maintenance

## Monitoring 🕵️

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

## Backup maintenance 🧹

If a crate version is deleted from the crates-io bucket (e.g. for GDPR reasons), an admin needs to delete it from the GCP bucket as well.
Even though the delete will propagate to GCP, the `soft-delete` feature will preserve the data, so we need to delete it manually.
