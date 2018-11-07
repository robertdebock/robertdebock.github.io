---
title: Ansible Molecule testing on EC2
---

# Ansible Molecule testing on EC2

[Molecule] is great to test [Ansible] roles, but testing locally with has it's limitations:

- [Docker] - Not everything is possible in Docker, like starting services, rebooting of working with block devices.
- [Vagrant] - Nearly everything is possible, but it's resource intensive, making testing slow.

I use my bus-ride time to develop Ansible Roles and the internet connection is limited, which means a lot of waiting. Using AWS EC2 would solve a lot of problems for me.

Here is how to add an EC2 scenario to an existing role.

## Save AWS credentials
Edit `~/.aws/credentials` using information downloaded from [AWS Console].

```
[default]
aws_access_key_id=ABC123
aws_secret_access_key=ABC123
```

## Install extra software
On the node where you initiate the tests, a few extra pip modules are required.

```
pip install boto boto3
```

## Add a scenario
If you already have a role and want to add a single scenario:

```
cd ansible-role-your-role
molecule init scenario --driver-name ec2 --role-name ansible-role-your-role --scenario-name ec2 --driver-name ec2
```

## Start testing
And simply start testing in a certain region.

```
export EC2_REGION=eu-central-1
molecule test --scenario-name ec2
```
