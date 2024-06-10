---
title: Improvements to the Ansible Automation Platform installer
---

# Improvements to the Ansible Automation Platform installer

If you have ever installed Tower or [Ansible Automation Platform](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.4/html-single/red_hat_ansible_automation_platform_installation_guide/index), you may know that it's pretty specific. Here are my suggested improvements to the installer.

## 1. Output is cluttered

The output of the installer does not use [loop_control.label](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html#limiting-loop-output-with-label) causing the output to look something like this:

```text
TASK [ansible.automation_platform_installer.repo_management : Install the Automation Platform bundle repository] ***
changed: [myaap-1.example.com] => {"changed": true, "checksum": "180eafb0ddf80e87f464c359358238a8aed1374b", "dest": "/etc/yum.repos.d/ansible-automation-platform.repo", "gid": 0, "group": "root", "md5sum": "36032c7104c2c8bfa11093eae71f54cb", "mode": "0644", "owner": "root", "secontext": "system_u:object_r:system_conf_t:s0", "size": 268, "src": "/root/.ansible/tmp/ansible-tmp-1718028961.09427-63855-258049482086701/source", "state": "file", "uid": 0}
changed: [myaap-0.example.com] => {"changed": true, "checksum": "180eafb0ddf80e87f464c359358238a8aed1374b", "dest": "/etc/yum.repos.d/ansible-automation-platform.repo", "gid": 0, "group": "root", "md5sum": "36032c7104c2c8bfa11093eae71f54cb", "mode": "0644", "owner": "root", "secontext": "system_u:object_r:system_conf_t:s0", "size": 268, "src": "/root/.ansible/tmp/ansible-tmp-1718028961.074965-63853-127923363074838/source", "state": "file", "uid": 0}
changed: [myaap-database.example.com] => {"changed": true, "checksum": "180eafb0ddf80e87f464c359358238a8aed1374b", "dest": "/etc/yum.repos.d/ansible-automation-platform.repo", "gid": 0, "group": "root", "md5sum": "36032c7104c2c8bfa11093eae71f54cb", "mode": "0644", "owner": "root", "secontext": "system_u:object_r:system_conf_t:s0", "size": 268, "src": "/root/.ansible/tmp/ansible-tmp-1718028961.0822551-63852-33509642687031/source", "state": "file", "uid": 0}
changed: [myaap-2.example.com] => {"changed": true, "checksum": "180eafb0ddf80e87f464c359358238a8aed1374b", "dest": "/etc/yum.repos.d/ansible-automation-platform.repo", "gid": 0, "group": "root", "md5sum": "36032c7104c2c8bfa11093eae71f54cb", "mode": "0644", "owner": "root", "secontext": "system_u:object_r:system_conf_t:s0", "size": 268, "src": "/root/.ansible/tmp/ansible-tmp-1718028961.079015-63857-51597012965829/source", "state": "file", "uid": 0}
```

As a consumer of this installer, I do not need all the clutter.

## 2. Some tasks are allowed to fail

```text
TASK [ansible.automation_platform_installer.config_dynamic : Check if node is registered with RHSM] ***
fatal: [myaap-0.example.com]: FAILED! => {"changed": true, "cmd": ["subscription-manager", "identity"], "delta": "0:00:01.056821", "end": "2024-06-10 14:12:26.504043", "msg": "non-zero return code", "rc": 1, "start": "2024-06-10 14:12:25.447222", "stderr": "This system is not yet registered. Try 'subscription-manager register --help' for more information.", "stderr_lines": ["This system is not yet registered. Try 'subscription-manager register --help' for more information."], "stdout": "", "stdout_lines": []}
...ignoring
```

This one can be solved by setting `failed_when` to `false` and using a `when` condition in a follow-up task.

Or this one:

```text
TASK [ansible.automation_platform_installer.preflight : Preflight check - Read in controller version] ***
fatal: [myaap-1.example.com]: FAILED! => {"changed": false, "msg": "file not found: /var/lib/awx/.tower_version"}
...ignoring
```

This task should be preceded with a `ansible.builtin.stat` task and only attempt to run the described task when the file exists.

## 3. Command used too often

Some tasks use the `ansible.builtin.command` module, where a "real" Ansible module is available. For example:

```text
TASK [ansible.automation_platform_installer.redis : Enable Redis module] *******
changed: [myaap-2.example.com] => {"changed": true, "cmd": ["dnf", "module", "-y", "enable", "redis:6"], "delta": "0:00:04.711806", "end": "2024-06-10 14:18:47.991515", "msg": "", "rc": 0, "start": "2024-06-10 14:18:43.279709", "stderr": "", "stderr_lines": [], "stdout": "Updating Subscription Management repositories.\nUnable to read consumer identity\n\nThis system is not registered with an entitlement server. You can use subscription-manager to register.\n\nLast metadata expiration check: 0:00:06 ago on Mon 10 Jun 2024 02:18:37 PM UTC.\nDependencies resolved.\n================================================================================\n Package           Architecture     Version             Repository         Size\n================================================================================\nEnabling module streams:\n redis                              6                                          \n\nTransaction Summary\n===================================================
```

Here the module [community.general.dnf_config_manager](https://docs.ansible.com/ansible/latest/collections/community/general/dnf_config_manager_module.html) could have been used, making the task idempotent.

There are likely some tasks that require `ansible.builtin.command` in which case `changed_when` can be used to assert if a change was done or not.

## 4. Inconsistent names are uses

Some tasks end with a period, some do not. Here is an example:

```text
TASK [ansible.automation_platform_installer.repo_management : Remove legacy ansible-tower repository] ***
TASK [ansible.automation_platform_installer.repo_management : Install the Automation Platform yum repository.] ***
```

## 5. Invalid (inventory) configuration are possible.

The Ansible Automation Platform setup used an [inventory](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.4/html-single/red_hat_ansible_automation_platform_installation_guide/index#con-install-scenario-recommendations) to explain what the installer should do. There are many invalid configuration possible. This means the installer will start to configure the instances, but fail with an "abstract" error at some moments.

## Conclusion

The installer of Ansible Automation Platform does work, there are many interesting patterns used, but there is also room for improvement. As this installer feels a bit like a show-case for Ansible, I do not like the fragility of this installer.
