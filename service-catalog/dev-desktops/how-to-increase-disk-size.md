# How to Increase the Disk Size

The [dev-desktops] are powerful, cloud-based servers that can be used by
maintainers to work on the [Rust Project].

Working on the Rust compiler requires a lot of disk space, with each checkout
typically requiring around 50GB of space. When users want to work on multiple
features in parallel or test multiple versions next to each other, they may need
up to 150-200GB of space for themselves.

Given that the number of users on the [dev-desktops] is constantly growing, once
in a while we need to increase the size of the disks on the server. We try to
find a good balance between size and cost, given that we have selected faster
but more expensive SSDs for these servers.

The guide is split into three steps:

1. First, we will increase the size of the disk that is attached to the server.
2. Second, we will increase the size of the filesystem to make use of the
   additional space.
3. Third, we will update the Terraform configuration to match the new state.

These steps slightly differ by the cloud provider, so the guide has
instructions for both AWS and Azure.

## AWS

On AWS, the first step is to increase the size of the virtual hard drive that is
attached to the server.

### Increase an AWS EBS Volume

We use [EBS](https://aws.amazon.com/ebs/) volumes for the disks. These are very
flexible, easy to use, and can be resized while the server is running. For the
[dev-desktops], we use the `gp3` volume type.

The easiest way to resize the disks is to go into the AWS Console, modify the
size through the UI, and then update the Terraform configuration to match the
new state.

The process is documented in more detail here:
<https://docs.aws.amazon.com/ebs/latest/userguide/requesting-ebs-volume-modifications.html>

- Sign in to the SSO portal at <https://rust-lang.awsapps.com/start/>
- Open the `Rust Admin - 8450` legacy account that contains the dev-desktops
- Navigate to the EC2 service and click on `Volumes` in the menu on the left
- Make sure you are in the right AWS region
- Find the volume that you want to resize and click on it
- Click `Modify` in the top right corner
- Change the size of the volume to the desired size
- Click `Modify` to apply the changes
- Confirm the dialog and end up back on the volume details page

After these steps, it is important to wait until the volume enters the
`Optimizing` state. This can take a few minutes.

### Increase the Filesystem

After increasing the size of the virtual hard drive, we need to increase the
size of the filesystem to make use of the additional space.

More documentation for this process can be found at <https://docs.aws.amazon.com/ebs/latest/userguide/recognize-expanded-volume-linux.html>.

First, list all disks and identity the one that you want to resize:

```shell
$ lsblk
NAME         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
# ... lots of loop devices ...
nvme0n1      259:0    0    2T  0 disk
├─nvme0n1p1  259:1    0    1T  0 part /
└─nvme0n1p15 259:2    0   99M  0 part /boot/efi
```

In this case, we want to resize `/dev/nvme0n1p1`. We can use the `growpart`
command to resize the partition `1` on the disk:

```shell
growpart /dev/nvme0n1 1
```

Running `lsblk` again will confirm that the partition now uses the full size of
the disk:

```shell
$ lsblk
NAME         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
nvme0n1      259:0    0    2T  0 disk
├─nvme0n1p1  259:1    0    2T  0 part /
└─nvme0n1p15 259:2    0   99M  0 part /boot/efi
```

## Azure

On Azure, only one step is necessary since the filesystem is automatically
resized when the machines boot.

### Increase an Azure Managed Disk

We use [Managed Disks](https://learn.microsoft.com/en-us/azure/virtual-machines/managed-disks-overview)
for the disks. We have chosen the `Standard SSD` type with locally-redundant
storage (LRS).

Detailed documentation on hwo to increase the disks on Azure can be found here:
<https://learn.microsoft.com/en-us/azure/virtual-machines/linux/expand-disks?tabs=ubuntu>

The easiest way to resize the disks is to use the [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli).
Install the CLI, log in using `az login`, and follow the steps below.

For convenience, you can export the name of the resource group and the machine
that you want to resize:

```shell
export RESOURCE_GROUP=dev-desktops-prod
export DEV_DESKTOP=dev-desktop-us-2
```

We first need to deallocate the machine before we can resize the disk:

```shell
az vm deallocate --resource-group "${RESOURCE_GROUP}" --name "${DEV_DESKTOP}"
```

After the machine has been stopped and deallocated, we can resize the disk. The
disks are named just like the machines, so we can reuse the environment
variable. Make sure to change the size to the desired value:

```shell
az disk update --resource-group "${RESOURCE_GROUP}" --name "${DEV_DESKTOP}" --size-gb 2048
```

After the disk has been resized, we can start the machine again:

```shell
az vm start --resource-group "${RESOURCE_GROUP}" --name "${DEV_DESKTOP}"
```

The filesystem is automatically resized when the machine boots. This can be
confirmed by running `df` and checking the size of the `/dev/root` partition:

```shell
$ df -Th
Filesystem     Type      Size  Used Avail Use% Mounted on
/dev/root      ext4      2.0T  949G  1.1T  48% /
# ...
```

## Update Terraform

After making changes to the disks, either through the web interface or the CLI,
we need to update the Terraform configuration to match the new state.

The configuration for the AWS-based machines can be found in the `terraform`
directory in [rust-lang/simpleinfra]:

<https://github.com/rust-lang/simpleinfra/blob/master/terraform/dev-desktops/regions.tf>.

The Azure-based machines have already been migrated to Terragrunt. Their
configuration is split by region, so multiple files might need to be updated:

- <https://github.com/rust-lang/simpleinfra/blob/master/terragrunt/accounts/dev-desktops-prod/westeurope/terragrunt.hcl>
- <https://github.com/rust-lang/simpleinfra/blob/master/terragrunt/accounts/dev-desktops-prod/westus2/terragrunt.hcl>

Update the `storage` variable to match the new size of the disks, and then run
`terraform plan` or `terragrunt plan` to ensure that no changes are detected.
This confirms that the configuration matches the current state.

[dev-desktops]: ./README.md
[rust project]: https://rust-lang.org
[rust-lang/simpleinfra]: https://github.com/rust-lang/simpleinfra
