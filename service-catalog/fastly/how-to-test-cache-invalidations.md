# How to Test Cache Invalidations

We use [Fastly] to cache Rust releases and crates. Both are cached with a long
time-to-live (TTL) to improve the cache hit ratio and reduce our costs for
outbound traffic from S3. However, we need to invalidate the cache when new
versions of Rust or crates are released. This is done through Fastly's API as
part of the release and publish processes.

Sometimes, it is necessary to test whether the cache invalidation works as
expected. This document outlines the steps that need to be taken.

## Prerequisites

## Find an Artifact

The first step is finding a suitable artifact for the test.

1. Go to <https://rust-lang.awsapps.com/start/> and log into the AWS console.
2. Open the legacy account (`Rust Admin - 8450`) and navigate to the S3 service.
3. Open the `dev-static-rust-lang-org` bucket and go into the `rustup` folder.
4. Find a file that is not frequently updated, e.g. one in the archive.

For the rest of the guide, we will be working with the checksum file
`/rustup/archive/1.27.0/x86_64-unknown-linux-gnu/rustup-init.sha256`.

## Download the Artifact

The second step is downloading the artifact to ensure it is in the cache. This
can be done by fetching the file from the Fastly service:

```shell
curl -I https://fastly-dev-static.rust-lang.org/rustup/archive/1.27.0/x86_64-unknown-linux-gnu/rustup-init.sha256
```

Make sure that the `x-cache` header is a `HIT`. This indicates that the file is
cached in Fastly's network. Check the `age` header and note down the age of the
file.

After invalidating the cache, we expct the `x-cache` header to be a `MISS` and
the `age` header to be `0`.

## Purge the Cache

The third step is purging the cache. We use two different methods to purge the
cache: [surrogate keys] for Rust releases and [URL purges] for crates. Follow
the respective instructions below.

### Purge a Surrogate Key

Purging a surrogate key invalidates all objects that are tagged with the key.
Which objects get tagged with which key is defined in the VCL configuration for
the Fastly service, which can be found in [`terragrunt/modules/release-distribution/fastly-static.tf`]
in [rust-lang/simpleinfra].

For this example, we will be purging a rustup artifact using the `rustup` key.

For convenience, we export the Fastly service ID as an environment variable.
Change this if you are working on a different service or environment.

```shell
# dev-static.rust-lang.org
export FASTLY_SERVICE_ID=5qaYFyyiorVua6uCZg7It0
```

We also need a Fastly authentication token. Replace `<YOUR_FASTLY_TOKEN>` with
the actual token.

```shell
export FASTLY_AUTH_TOKEN=<YOUR_FASTLY_TOKEN>
```

With a valid authentication token from Fastly, use the following command to send
a [purge request](https://www.fastly.com/documentation/reference/api/purging/#purge-tag)
for the key.

```shell
curl -i -X POST "https://api.fastly.com/service/${FASTLY_SERVICE_ID}/purge/rustup" \
  -H "Fastly-Key: ${FASTLY_AUTH_TOKEN}" \
  -H "Accept: application/json"
```

We expect the response to have a `200 OK` status code and return an ID for the
purge in its body:

```json
{ "status": "ok", "id": "8230126-1715084554-7370607" }
```

### Purge a URL

The simplest way to purge an object on Fastly is by purging its URL.

Export the URL that you want to purge as an environment variable:

```shell
export URL_TO_PURGE=fastly-dev-static.rust-lang.org/rustup/archive/1.27.0/x86_64-unknown-linux-gnu/rustup-init.sha256
```

We also need a Fastly authentication token. Replace `<YOUR_FASTLY_TOKEN>` with
the actual token and export it as an environment variable for convenience.

```shell
export FASTLY_AUTH_TOKEN=<YOUR_FASTLY_TOKEN>
```

With a valid authentication token from Fastly, use the following command to send
a [purge request](https://www.fastly.com/documentation/reference/api/purging/#purge-single-url)
for the URL.

```shell
curl -i -X POST "https://api.fastly.com/purge/${URL_TO_PURGE}" \
  -H "Fastly-Key: ${FASTLY_AUTH_TOKEN}" \
  -H "Accept: application/json"
```

We expect the response to have a `200 OK` status code and return an ID for the
purge in its body:

```json
{ "status": "ok", "id": "8230126-1715084554-7370607" }
```

## Verify Purge

After purging the cache, we can verify that the cache has been invalidated by
fetching the file again:

```shell
curl -I https://fastly-dev-static.rust-lang.org/rustup/archive/1.27.0/x86_64-unknown-linux-gnu/rustup-init.sha256
```

The `x-cache` header should now be a `MISS` and the `age` header should be `0`.

[`terragrunt/modules/release-distribution/fastly-static.tf`]: https://github.com/rust-lang/simpleinfra/blob/master/terragrunt/modules/release-distribution/fastly-static.tf
[rust-lang/simpleinfra]: https://github.com/rust-lang/simpleinfra
[surrogate keys]: https://www.fastly.com/documentation/guides/concepts/edge-state/cache/purging/#surrogate-key-purge
[url purges]: https://www.fastly.com/documentation/guides/concepts/edge-state/cache/purging/#url-purge
[Fastly]: ./README.md
