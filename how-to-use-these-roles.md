## [How to use these roles](#how-to-use-these-roles)

There is [quite](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html) [some](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html) [documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html) available already, but it can't hurt to briefly explain how to use these roles.

## [My promise](#my-promise)

- The roles work on as many distribution as possible. The intent is that you can switch the operating system, without changing your playbook or variables. There are exceptions, for example the Zabbix roles, as Zabbix is only provided for a few operating systems.
- These roles are as simple as possible. There can be cases where complicated stuff happens, but that's required from time to time.
- These roles are thoroughly tested. On each commit and every month, in containers on GitHub Actions and GitLab CI, see [testing](testing.html).

## [Installing the roles](#installing-the-roles)

Before using any role, they need to be installed. There are a few ways to do that:

1. Install them one-by-one using `ansible-galaxy role install robertdebock.bootstrap` for example.
2. Install them using a `requirements.yml` file (see below) using `ansible-galaxy role install --role-file requirements.yml`.

The `requirements.yml` file is a list of roles, for example:

```yaml
---
- robertdebock.bootstrap
```

These roles are typically installed in `~/.ansible/roles/`. You can configure Ansible to look somewhere else though, for example this `ansible.cfg` stores the roles "locally", in the same directory where the ansible.cfg is found:

```ini
[defaults]
roles_path = roles
```

## [Including the roles in playbooks](#including-the-roles-in-playbooks)

Once the roles are downloaded, you can integrate them into a playbook, for example:

```yaml
---
- name: bootstrap all hosts
  hosts: all
  become: true
  gather_facts: false

  roles:
    - role: robertdebock.bootstrap
```

You can also use [`include_role`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/include_role_module.html).

## [Using variables](#using-variables)

Most roles can be controlled using [variables](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html). For example by using the `vars:` statement:

```yaml
---
- name: bootstrap all hosts
  hosts: all
  become: true
  gather_facts: false


  roles:
    - role: robertdebock.bootstrap
      bootstrap_wait_for_host: true
```

Most roles have quite a few variables and the playbook can get long using the example above. Another way to use variables in in the [inventory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html), specifically in the [group variables](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html). For example the file `group_vars/all.yml` can contain:

```yaml
---
bootstrap_wait_for_host: true
```

## [Full example](#full-example)

This is a layout of the file and directory structure that I tend to use:

```text
├── ansible.cfg
├── inventory
│   ├── group_vars
│   │   └── all.yml
│   └── hosts
├── playbook.yml
└── roles
    ├── requirements.yml
    └── robertdebock.bootstrap
```

Also see [best practices](https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html).

### [ansible.cfg](#ansible-cfg)

```ini
[defaults]
roles_path=roles
retry_files_enabled=no
inventory=inventory
```

Also see [Ansible configuration](https://docs.ansible.com/ansible/latest/reference_appendices/config.html).

### [inventory/hosts](#inventory-hosts)

```text
server-01 ansible_host=192.0.2.10
```

Also see [working with inventories](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html).

### [inventory/group_vars/all.yml](#inventory-group-vars-all-yml)

```yaml
---
bootstrap_wait_for_host: true
```

Also see [group variables](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html)

### [playbook.yml](#playbook-yml)

```yaml
---
- name: bootstrap all hosts
  hosts: all
  become: true
  gather_facts: false

  roles:
    - robertdebock.bootstrap
```

Also see [working with playbooks](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html).

### [roles/requirements.yml](#roles-requirements-yml)

```yaml
---
- robertdebock.bootstrap
```

Also see [installing roles from a file](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#installing-multiple-roles-from-a-file).

### [roles/robertdebock.bootstrap](#roles-robertdebock-bootstrap)

Although a consumer of the role does not need to look into the role and `ansible-galaxy install` will manage the contents of these roles, here is a brief explanation what can be found here:

- `defaults/main.yml` - Variables that can be overwritten by you, the consumer of the role.
- `tasks/main.yml` - A list of tasks that this role executes.
- `vars/main.yml` - Variables that should not be overwritten by you. For example a mapping from operating system and the packages that should be installed.
- `templates/*` - Files that are generated on the managed node, these typically contain variable that are substituded for a value. The files in here should be mentioned in the `tasks/main.yml`.
- `molecule/*` - A few scenarios that the role is tested against. I typically see a scenario as a distribution.
- `files/*` - A set of static files. The files in here should be mentioned in the `tasks/main.yml`.
- `handlers/main.yml` - Tasks that are executed after the play when called from a task in `tasks/main.yml` when the task is changed.
- `meta/main.yml` - Descriptive information about the role.
- `README.md` - Documentation how to use the role.

Also see [Ansible roles](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html).
