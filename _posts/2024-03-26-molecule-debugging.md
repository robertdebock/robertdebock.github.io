---
title: Molecule debugging
---

# Molecule debugging

Using [Molecule](https://ansible.readthedocs.io/projects/molecule/) really helps to develop Ansible roles. (and collections.) But, when things don't go as planned, it can be a little more challenging to understand what's wrong.

When Molecule runs, you see the Ansible output, something like this:

```text
TASK [ansible-role-auditd : Start and enable auditd] ***************************
fatal: [auditd-fedora-rawhide]: FAILED! => {"changed": false, "msg": "A dependency job for auditd.service failed. See 'journalctl -xe' for details.\n"}
```

## Now what

There are a couple of options:

1. You can login to the instance Molecule created. You need to let the instance exist by running `molecule test --destroy=never`. Next you can run `molecule login` to jump into the instance Molecule was running against. You can now troubleshoot the instance.
2. You can put Ansible in debug mode: `ANSIBLE_DEBUG=True molecule test`.
3. You can increase verbosity: `ANSIBLE_VERBOSITY=5 molecule test`.
4. You can put Ansible in diff mode: `DIFF_ALWAYS=True molecule test`.
5. You can leave Ansible created scripts: `ANSIBLE_KEEP_REMOTE_FILES=True molecule test`. This allows you to inspect the scripts Ansible placed on the target.

In my case `ANSIBLE_VERBOSIY` is set to `5`: `ANSIBLE_VERBOSITY=5 molecule test`:

```text
TASK [ansible-role-auditd : Start and enable auditd] ***************************
task path: /Users/robertdb/Documents/github.com/robertdebock/ansible-role-auditd/tasks/main.yml:44
Running ansible.legacy.service

...

fatal: [auditd-fedora-rawhide]: FAILED! => {
    "changed": false,
    "invocation": {
        "module_args": {
            "arguments": "",
            "enabled": true,
            "name": "auditd",
            "pattern": null,
            "runlevel": "default",
            "sleep": null,
            "state": "started"
        }
    },
    "msg": "A dependency job for auditd.service failed. See 'journalctl -xe' for details.\n"
}

PLAY RECAP *********************************************************************
auditd-fedora-rawhide      : ok=42   changed=0    unreachable=0    failed=1    skipped=3    rescued=0    ignored=0
```

(Still no clue what's wrong by the way, but at least I see more of what Ansible is trying to do.)

Hope this helps you get unblocked with your Ansible/Molecule issue!
