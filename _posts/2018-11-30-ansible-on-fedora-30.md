---
title: Ansible on Fedora 30
---

# Ansible on Fedora 30.

[Fedora 30](https://fedoraproject.org/wiki/Releases/30/Schedule) (currenly under development as [rawhide](https://fedoraproject.org/wiki/Releases/Rawhide)) does [not have python2-dnf](https://fedoraproject.org/wiki/Releases/30/ChangeSet#Mass_Python_2_Package_Removal) anymore.

The [Ansible module dnf](https://docs.ansible.com/ansible/latest/modules/dnf_module.html) tries to install python2-dnf if it running on a python2 environment. It took me quite some time to figure out why [this error](https://travis-ci.org/robertdebock/ansible-role-bootstrap/jobs/461449416) appeared:

```
fatal: [bootstrap-fedora-rawhide]: FAILED! => {"attempts": 10, "changed": true, "msg": "non-zero return code", "rc": 1, "stderr": "Error: Unable to find a match\n", "stderr_lines": ["Error: Unable to find a match"], "stdout": "Last metadata expiration check: 0:01:33 ago on Thu Nov 29 20:16:32 2018.\nNo match for argument: python2-dnf\n", "stdout_lines": ["Last metadata expiration check: 0:01:33 ago on Thu Nov 29 20:16:32 2018.", "No match for argument: python2-dnf"]}
```

(I was not trying to install python2-dnf, so confusion...)

Hm; so I've tried these options to work around the problem:

- Use [alternatives](https://fedoraproject.org/wiki/Alternatives_system) to set /usr/bin/python to /usr/bin/python3. Does not work, the Ansible module dnf will still try to install python2-dnf.
- Set `ansible_python_interpreter` for Fedora-30 hosts. Does not work, my [bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap) role does not have any facts, it does not know about `ansible_distribution` (`Fedora`), nor `ansible_distribution_major_version` (`30`).

so far the only reasonable option is to set `ansible_python_interpreter` as [documented by Ansible](ansible_python_interpreter). Hm, so my [molecule.yml](https://github.com/robertdebock/ansible-role-bootstrap/blob/master/molecule/fedora-rawhide/molecule.yml) now contains this code to tell Ansible to use python3:

```
provisioner:
  name: ansible
  inventory:
    group_vars:
      all:
        ansible_python_interpreter: /usr/bin/python3
```

This means all roles that use distributions that:
- use dnf
- don't have pyton2-dnf

will need to be modified... Quite a change.

2 December 2018 update: I've created [pull request 49202](https://github.com/ansible/ansible/pull/49402) to fix [issue 49362](https://github.com/ansible/ansible/issues/49362).

TL;DR On Fedora 30 (and higher) you have to set `ansible_python_interpreter` to `/usr/bin/python3`.
