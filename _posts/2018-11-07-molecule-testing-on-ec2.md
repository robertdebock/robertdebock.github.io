---
title: Ansible Molecule testing on EC2
---

# Ansible Molecule testing on EC2

[Molecule](https://molecule.readthedocs.io/en/latest/) is great to test [Ansible](https://www.ansible.com/) roles, but testing locally with has it's limitations:

- [Docker](https://www.docker.com/) - Not everything is possible in Docker, like starting services, rebooting of working with block devices.
- [Vagrant](https://www.vagrantup.com/) - Nearly everything is possible, but it's resource intensive, making testing slow.

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

The molecule.yml should look something like this:
```
---
dependency:
  name: galaxy
driver:
  name: ec2
lint:
  name: yamllint
platforms:
  - name: rhel-7
    image: ami-c86c3f23
    instance_type: t2.micro
    vpc_subnet_id: subnet-0e688067
  - name: sles-15
    image: ami-0a1886cf45f944eb1
    instance_type: t2.micro
    vpc_subnet_id: subnet-0e688067
  - name: amazon-linux-2
    image: ami-02ea8f348fa28c108
    instance_type: t2.micro
    vpc_subnet_id: subnet-0e688067
provisioner:
  name: ansible
  lint:
    name: ansible-lint
scenario:
  name: ec2
```

## Weirdness
It feels as if the ec2 driver has had a little less attention as for example the vagrant or docker driver. Here are some strange things:
- The region needs to be set using an environment variable, the credentials from a file. This may be my mistake, but now it's a little strange. It would feel more logical to add `region:` to the `platform` section.
- The `vpc_subnet_id` should be found by the user and put into `molecule.yml`.
