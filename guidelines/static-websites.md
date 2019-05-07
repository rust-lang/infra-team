# Rust Infrastructure hosting for static websites

The Rust Infrastructure team provides hosting for static websites available for
all Rust teams. This document explains the requirements a website needs to meet
and how to setup one.

## Requirements for hosting websites

* The website must be managed by a Rust team, or be officially affiliated with
  the project.
* The website’s content and build tooling must be hosted on a GitHub
  repository in either the [rust-lang](https://github.com/rust-lang) or
  [rust-lang-nursery](https://github.com/rust-lang-nursery) organizations.
* The website must be built and deployed with Travis CI.
* The website must reach an A+ grade on the
  [Mozilla Observatory](https://observatory.mozilla.org/).
* The website can be hosted on either GitHub Pages or Amazon S3 (in the
  rust-lang account), and must sit behind CloudFront.

## Adding custom headers

One of the requirements for having a static website hosted by the
infrastructure team is to reach an A+ grade on the [Mozilla
Observatory](https://observatory.mozilla.org/), and that requires custom
headers to be set. Unfortunately neither GitHub Pages nor Amazon S3 allows
setting up them, but we have pieces of infrastructure setup to work around the
limitation.

To setup custom headers a file named `_rustinfra_config.json` needs to be
included in the generated website. This example content includes all the
headers needed to reach grade B on the Observatory (to reach grade A+ a Content
Security Policy needs to be added):

```json
{
    "headers": {
        "Strict-Transport-Security": "max-age=63072000",
        "X-Content-Type-Options": "nosniff",
        "X-Frame-Options": "DENY",
        "X-XSS-Protection": "1; mode=block",
        "Referrer-Policy": "no-referrer, strict-origin-when-cross-origin"
    }
}
```

If you’re using Jekyll as the build tool for the website you also need to
explicitly whitelist the file in the `_config.yml`, otherwise Jekyll will
ignore it since it starts with a `_`:

```yaml
include:
  - _rustinfra_config.json
```

## Deployment guide

### Configuring AWS

Create a CloudFront web distribution and set the following properties:

- **Origin Domain Name:** rust-lang.github.io/repo-name
- **Origin Protocol Policy:** HTTPS Only
- **Viewer Protocol Policy:** Redirect HTTP to HTTPS
- **Lambda Function Association:**
    - **Viewer Response:** arn:aws:lambda:us-east-1:890664054962:function:static-websites:1
- **Alternate Domain Names:** your-subdomain-name.rust-lang.org
- **SSL Certificate:** Custom SSL Certificate
    - You will need to request the certificate for that subdomain name through ACM
- **Comment:** your-subdomain-name.rust-lang.org

Wait until the distribution is propagated and take not of its `.cloudfront.net`
domain name.

Head over to the domain’s Route 53 hosted zone and create a new record set:

- **Name:** your-subdomain-name
- **Type:** CNAME
- **Value:** the `.cloudfront.net` domain name you saw earlier

Create an AWS IAM user to allow the CI provider used to deploy website changes
to perform whitelisted automatic actions. Use `ci--ORG-NAME--REPO-NAME` (for
example `ci--rust-lang--rust`) as the user name, allow programmatic access to
it and add it to the `ci-static-websites` IAM group. Then take note of the
access key id and the secret access key since you’ll need those later.

### Deploying the website

To deploy websites we don’t use GitHub tokens (since they don’t have granular
access scoping) but a deploy key with write access unique for each repository.
To setup the deploy key you need to be an administrator on the repository,
clone the [simpleinfra](https://github.com/rust-lang/simpleinfra) repository
and run this command:

```
$ cargo run --bin setup-deploy-keys rust-lang/repo-name
```

The command requires the `GITHUB_TOKEN` ([you can generate one
here](https://github.com/settings/tokens)) and the `TRAVIS_TOKEN` ([you can see
yours here](https://travis-ci.com/account/preferences)) to be present. It will
generate a brand new key, upload it to GitHub and configure Travis CI to use
it.

To actually deploy the website this snippet needs to be added to your
`.travis.yml` (please replace the contents of `RUSTINFRA_DEPLOY_DIR` and
`RUSTINFRA_CLOUDFRONT_DISTRIBUTION`):

```yaml
env:
  RUSTINFRA_DEPLOY_DIR: path/to/be/deployed
  RUSTINFRA_CLOUDFRONT_DISTRIBUTION: ABCDEFGHIJKLMN
import:
  - rust-lang/simpleinfra/travis-configs/static-websites.yml
```

You will also need to set the contents of the `AWS_ACCESS_KEY_ID` and
`AWS_SECRET_ACCESS_KEY` environment variables on the Travis CI web UI with the
credentials of the IAM user you created earlier. The secret access key **must**
be hidden from the build log, while the access key id should be publicly
visible.
