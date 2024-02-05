---
title: How I setup sandboxes
---

# How I setup sandboxes

> Context: I work in projects for customers. Mostly involving Ansible, Terraform, GitLab, Vault and Terraform Enterprise.

At the start of a project I'll keep my eyes and ears open to understand the environment I will be working in. These details include:

- What infrastructure (VMWare, AWS, GCP, Azure, etc.)
- What services relate to the thing I'm helping with. (LDAP, GitLab, OpenShift, etc)
- How is the network segregated.
- What issues are there at the moment.

I'll mostly know what the deliverable is up-front. Sometimes it's difficult to visualize what is expected anyway, that's just something I'm not very good at.

Anyway, once I know **what** to do and know **where** to do it, I'll start drawing. Images like this, quite abstract:

![Rough Architecture](/images/rough-architecture.png)

This imaging gives me a clue how to start coding. Coding I'll mostly use Terraform to deploy infrastructure. It nearly always ends up with some instances, a load-balancer and everything required to support the application.

When using instances, It's quite annoying that the IP/hostname keeps changing every deployment. So I'll let Terraform write an ssh-configuration file.

First; I'll include a `conf.d` directory from `~/.ssh/config`:

```text
Include conf.d/*
```

(And just to be explicit, this is done manually, once, not by Terraform.)

Next I'll let Terraform write an ssh configuration using a template (`templates/ssh_config.tpl`):

```text
%{ for index, instance in instances ~}
Host instance-${ index }
  HostName ${ instance }
  User ec2-user
  IdentityFile ${ ssh_key_path }

%{ endfor }
```

In the above example; a loop is created using `instances`. The `index` is used to count in what loop we're in.

Next, I'll let Terraform place the `~/.ssh/conf.d/SOME_PROJECT.conf` file:

```text
# Place the SSH configuration.
resource "local_file" "ssh_config" {
  content = templatefile("${path.module}/templates/ssh_config.tpl", {
    ssh_key_path = local_sensitive_file.default.filename
    instances    = aws_instance.default[*].public_ip
  })
  filename = pathexpand("~/.ssh/config")
}
```

You see that both `ssh_key_path` and `instances` are sent to the template. And what's missing here is all the code to make the instances and sensitive files.

Now if you `terraform apply`, Terraform will create instances and drop a .ssh config file, so you can simply type `ssh instance-0`. Also good for Ansible, because Ansible uses the .ssh configuration as well. So the inventory can simply contain the hostname `instance-0`. (And Terraform can create that inventory as well!)
