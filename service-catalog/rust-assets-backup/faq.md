# Rust Assets Backup: FAQ

## Do we need a multi-region backup for the object storage?

No. [Multi-region](https://cloud.google.com/storage/docs/availability-durability#cross-region-redundancy) only helps if we want to serve this data real-time and we want to have a fallback mechanism if a GCP region fails. We just need this object storage for backup purposes, so we don't need to pay more 👍

## Why did you choose the `europe-west1` GCP region?

It's far from the `us-west-1` region where the AWS S3 buckets are located. This protects us from geographical disasters.
The con is that the latency of the transfer job is higher when compared to a region in the US.
Also, the cost calculator indicates that this regions has a "Low CO2" and it's among the cheapest regions.

## Why GCP?

Both the Rust Foundation and the Rust project have a good working relationship with Google, and it is where the Rust Foundation's Security Initiative hosts its infrastructure.
Due to the good collaboration with Google, we expect that we can cover the costs of the backup with credits provided by Google.
