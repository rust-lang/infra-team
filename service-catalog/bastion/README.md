# Bastion

Bastion is an EC2 deployed in the `us-west-1` AWS region,
used to access other servers inside our VPC.

You can learn more about what a bastion is on
[Wikipedia](https://en.wikipedia.org/wiki/Bastion_host)

It is managed in the
[terraform/bastion](https://github.com/rust-lang/simpleinfra/blob/master/terraform/bastion/README.md)
module.

## Versions

We need to keep the Ubuntu version of the EC2 up-to-date.
