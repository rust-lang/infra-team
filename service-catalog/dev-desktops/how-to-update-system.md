# How to Update System

Periodically, it is necessary to update the packages on the dev desktops as well
as the operating system itself.

## Update Packages

Updating the packages is a straightforward process. Simply run the following
commands to first update the package index and then upgrade the packages:

```shell
sudo apt update
sudo apt upgrade
```

We have overwritten the `motd` configuration, which might cause a prompt. Answer
the prompt with `N` (the default) to keep our configuration.

After the packages have been updated, it is a good idea to reboot the machine to
ensure that all services are running with the latest versions:

```shell
sudo reboot now
```

Optionally, you can also run the following command to remove any packages that
are no longer needed:

```shell
sudo apt autoremove
```

## Update Ubuntu

To update the Ubuntu version:

1. Update the packages as described above.
2. Run `sudo do-release-upgrade` to upgrade to the next LTS version.
3. The system should reboot automatically after the upgrade.
4. Apply the Ansible playbook again to ensure it still works
5. Reboot the machine to ensure all services are running with the latest versions: `sudo reboot now`
