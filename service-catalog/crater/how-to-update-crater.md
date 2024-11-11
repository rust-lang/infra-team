# How to update crater

Crater is made of various components, here's how to update them.

> [!NOTE]
> Before updating crater check on [crater.rust-lang.org] if there are any running
> experiments. It's preferable to update crater when there are no running
> experiments.

## Agents

To update the ubuntu version of the agents, you need to update the
[agent template].

After you update ubuntu, verify that the agent are still reachable by the crater
server at in the [crater](https://crater.rust-lang.org/agents) website.

If the agents are not reachable, ssh into the agents VMs and run the command
`journalctl` to see what's wrong. You can find the VMs in the
[GCP console](https://console.cloud.google.com/compute/instances).

## Crater server

These are the versions we need to keep up-to-date:

- Ubuntu version specified in the [Dockerfile]
- Cargo dependencies: specified in the [Cargo.lock]
- GitHub Actions: specified in the [workflows] directory

Plus, update the Ubuntu version of the VM by ssh-ing into it (similarly to how
you would [update a dev-desktop](../dev-desktops/how-to-update-system.md)). The
reason for updating the same VM instead of replacing it, is that crater has a
SQLite database that would be lost if the VM was replaced.

[agent template]: https://github.com/rust-lang/simpleinfra/blob/74bbf479de315fb5c5d6e97832fc3dc9b12e4cab/terraform/crater/agent.tf#L139
[crater.rust-lang.org]: https://crater.rust-lang.org
[Dockerfile]: https://github.com/rust-lang/crater/blob/master/Dockerfile
[Cargo.lock]: https://github.com/rust-lang/crater/blob/master/Cargo.lock
[workflows]: https://github.com/rust-lang/crater/tree/master/.github/workflows
