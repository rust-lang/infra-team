# Rust Assets Backup: Maintenance

## Monitoring 🕵️

To check that the backups are working:

- Ensure the number of files and the size of the GCP buckets is the same as the respective AWS buckets by looking at the metrics
- Ensure that only the authorized people have access to the account

You can also run the following test:

- Upload a file in an AWS S3 bucket and check that it appears in GCP.
- Edit the file in AWS and check that you can recover the previous version from GCP.
- Delete the file in the AWS `crates-io` bucket and check that you can recover all previous versions from GCP.

In the future, we might want to create alerts in:

- _Datadog_: to monitor if the transfer job fails.
- _Wiz_: to monitor if the access control changes.

## Backup maintenance 🧹

### Deletions

The daily transfer mirrors deletions in the AWS `crates-io` bucket to its GCP backup bucket.
Deletions in `static-rust-lang-org` are not mirrored.

A mirrored delete doesn't remove data from the backup.
The GCP buckets have object versioning enabled, so the deleted object becomes a
noncurrent version that you can still read and restore.
Noncurrent versions are hidden from normal listings. To list them, run:

```sh
gcloud storage ls --all-versions 'gs://<bucket>/<object>'
```

### Retention

We keep every object version in the backup buckets forever, including objects
deleted from AWS because of GDPR requests or other takedowns.
This is intentional: it's a security retention that is exempt from GDPR.

- Never delete objects or object versions from the backup buckets, including for GDPR requests.
- Never add a lifecycle rule to the backup buckets.

If a version is deleted by mistake, soft delete keeps it for 90 days, and you can restore it during that time.
