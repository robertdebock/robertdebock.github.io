# Style guide

In general the [Ansible best practices](http://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html) are used.

Explicit mentions:
- Use `name:` where possible.
- Lowercase the value of the `name:` parameter.
- Prefer higher readability.
- Always mention `state:`

## Linting

All roles should pass [ansible-galaxy-rules](https://github.com/ansible/galaxy-lint-rules).

There are some roles that can't meet these requirements:

|---|---|
|role|reason for skip_ansible_lint|
|[common](https://galaxy.ansible.com/robertdebock/common)|[include_role does not support notify](https://github.com/ansible/ansible/issues/26537)|
|[reboot](https://galaxy.ansible.com/robertdebock/reboot)|?|
|[release](https://galaxy.ansible.com/robertdebock/release)|[include_role does not support notify](https://github.com/ansible/ansible/issues/26537)|
|[selinux](https://galaxy.ansible.com/robertdebock/selinux)|[include_role does not support notify](https://github.com/ansible/ansible/issues/26537)|
|[update]|(https://galaxy.ansible.com/robertdebock/update)|[include_role does not support notify](https://github.com/ansible/ansible/issues/26537) and [package should now use latest](https://github.com/ansible/galaxy-lint-rules/blob/master/rules/PackageIsNotLatestRule.py)|
