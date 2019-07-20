---
title: Why "hardening" is not a role
---

# Why "hardening" is not a role 

I see [many](https://github.com/openstack/ansible-hardening) [developers](https://github.com/dev-sec/ansible-os-hardening) [writing](https://github.com/konstruktoid/ansible-role-hardening) an Ansible role for `hardening`. Although these roles can absolutely be useful, here is why I think there is a better way.

Roles are (not always, but frequently) product centric. Think of role names like:
- auditd
- sshd
- users

A role for hardening you system has the potential to cover all kinds of topics that are covered in the product specific roles.

Besides that, in my opinion a role should be:
1. Small
2. Cover on function

A good indicator of a role that's too big is having multiple task files in `tasks`.

So my suggestion to not use a `harden` role, but rather have each role that you compose a system out of, use secure defaults.
