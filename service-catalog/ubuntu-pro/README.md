# Ubuntu Pro

[Ubuntu Pro] is Canonical's subscription for Ubuntu systems.

## Features

- It provides security patches for more packages than are covered by standard
  Ubuntu LTS security maintenance.
- It includes Livepatch, which applies kernel security fixes without requiring a
  reboot.
- It provides LTS security maintenance for up to 10 years. For example, Ubuntu
  18.04 LTS is supported by Ubuntu Pro until 2028.

Security updates are applied by the periodic `unattended-upgrades`.

> [!NOTE]
> ARM64 Livepatch coverage starts with Ubuntu 26.04.

You can learn more in the [Ubuntu Pro documentation].

## Donation

Canonical donated 25 Ubuntu Pro subscriptions to the Rust Foundation in June 2026,
which expire on 2027-06-12.

## Onboard a new admin to the dashboard

An existing admin can invite a new admin via the [Landscape](https://landscape.canonical.com/new_dashboard)
organization settings.

The new admin will receive an email invitation to join the organization, and they will be
able to login via [Ubuntu One](https://login.ubuntu.com/).

### Landscape

The Landscape is a centralized Ubuntu fleet management dashboard that allows to track what software
runs on each machine and whether it needs security updates.

We are not using the Landscape at the moment (it shows zero instances configured)
and there are no current plans to use it in the future (it's another installation process with
respect to ubuntu pro).

For more information, see the [Landscape docs](https://ubuntu.com/landscape).

## How to setup

### Manual setup

- Use the [Ubuntu Pro dashboard] to view subscriptions and access the Ubuntu
  Pro token.
- Attach the token to each Ubuntu machine with `sudo pro attach <YOUR_TOKEN>`.

> [!NOTE]
> Do not share the Ubuntu Pro token publicly.

> [!NOTE]
> For AWS, don't use the AWS License Manager process documented in the article
> [How to upgrade existing Ubuntu LTS instances to Ubuntu Pro in AWS](https://ubuntu.com/blog/upgrade-your-existing-ubuntu-lts-instances-to-ubuntu-pro-in-aws).
> Otherwise, we would pay for Ubuntu Pro with AWS credits.

### Ansible

If the machine is managed with Ansible, it uses Ubuntu Pro by default, thanks
to the [ubuntu-pro task](https://github.com/rust-lang/simpleinfra/blob/b0fc6ade93e07f391df24dbbee651061f9f7cd64/ansible/roles/common/tasks/ubuntu-pro.yml)
in the `common` role.

## How to verify setup

1. Run `pro status` and check that the services `esm-apps`, `esm-infra`, and `livepatch` are enabled.
2. (Optional) Run `pro security-status` to list available security updates for the system.

## Troubleshooting

### Kernel not covered by Livepatch

When running `pro status`, you might see a warning similar to this:

```
The current kernel (6.14.0-1018-aws, x86_64) is not covered by livepatch.
Covered kernels are listed here: https://ubuntu.com/security/livepatch/docs/kernels
```

To fix this, try to update the kernel by running the following:

```bash
sudo apt update
sudo apt full-upgrade
sudo reboot
```

[ubuntu pro]: https://ubuntu.com/pro
[ubuntu pro dashboard]: https://ubuntu.com/pro/dashboard
[ubuntu pro documentation]: https://documentation.ubuntu.com/pro/
