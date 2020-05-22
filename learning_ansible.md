# [Learning Ansible](#learning-ansible)

This is an as-short-as-possible walk through to learn Ansible. It should help you get going as quick as possible.

## Concept

[Ansible](https://www.ansible.com/) is a tool that can configure (remote) servers. It's simple to read, has many [modules](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html) and can run from a simple laptop.

With Ansible you describe what [tasks](#tasks) should be executed on selected remote hosts in the [inventory](#inventory).

## [Inventories](#inventories)

An [inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html) is a file that describes what servers should be managed. For example:

```ini
my_server.example.com

[my_webservers]
my_web_1.example.com
my_web_2.example.com

[amsterdam]
my_web_1.example.com

[london]
my_web_2.example.com

[singapore]
my_server.example.com

[europe:children]
amsterdam
london

[asia:children]
singapore
```

These [variables have a precedence](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable), for the group_vars:

1. `group_vars/all`
2. `group_vars/*`

This means more specific variables overwrite `all`.

In [playbooks](#playbooks) you can target [tasks](#tasks) and [roles](#roles) to run on selected hosts. In this case there are multiple groups, [2 are default](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#id6)

- `all` - A default group for any host mentioned in the inventory.
- `ungrouped` - Another default group for hosts in the inventory, but not in any group.
- `my_webservers` - This user-defined group can be seen in the inventory.
- `amsterdam`, `london` & `singapore` - Userdefined groups that contains 1 host.
- `europe` & `asia` - A group of group(s). Europe contains 2 cities.

You can make variables (like the `ntp_servers`) available [per group](#host_and_group_variables).

### [Host & Group variables](#host_and_group_variables)

Once you have an inventory defined, you can make certain variables available per host or group.

#### group_vars/all/ntp.yml

```yaml
ntp_servers:
  - name: pool.ntp.org
```

#### group_vars/amsterdam/ntp.yml

```yaml
ntp_servers:
  - name: nl.pool.ntp.org
```

#### host_vars/my_web_2.example.com.yml

```yaml
ntp_servers:
  - name: 1.uk.pool.ntp.org
```

With [inventories](#inventories) and [host & group variables](#host_and_group_variables) you can define all data required by [roles](#roles) to configure a system.

## [Tasks](#tasks)

A task is a statement of what Ansible should to, it's the smallest component in Ansible and you can use tasks in [playbooks](#playbooks), [roles](#roles) and [handlers](#handlers).

This task [copies](https://docs.ansible.com/ansible/latest/modules/copy_module.html) a file from the [Ansible Control node](https://docs.ansible.com/ansible/latest/network/getting_started/basic_concepts.html) to the server the task is targeted to.

```yaml
- name: Copy file with owner and permissions
  copy:
    src: /srv/myfiles/foo.conf
    dest: /etc/foo.conf
```

## [Roles](roles)

## [Playbooks](#playbooks)

A playbook is a list of [tasks](#tasks) and [roles](#roles) and some details to understand how to apply them.

```yaml
---
- name: configure my servers
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: show something
      debug:
        msg: "Hello world"

  roles:
    - role: robertdebock.ntp
```

There are a few parameters that need explaining:

- `hosts: all` - This is where a playbook is targeted against hosts. This is done using [patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html).
- `become: yes` - This tells Ansible to run the play as the root users. Although it's tempting to use `become: no` and choose when to use the root user when needed, for example per task, it's far easier to simply use `become: yes` at the play level like this example.
- `gather_facts` - Ansible should run the [`setup`](https://docs.ansible.com/ansible/latest/modules/setup_module.html) module that make [facts](#facts) available. Many tasks and roles benefit from these facts.

There [order or tasks, roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html?#using-roles) and others is:

1. `pre_tasks`
2. `roles`
3. `tasks`
4. `post_tasks`

## [Facts](#facts)

Ansible can get a lot of details from systems, called `facts`. A few examples of Ansible facts:

- `ansible_os_family: RedHat`
- `ansible_pkg_mgr: dnf`
- `ansible_distribution: Fedora`
- `ansible_distribution_major_version: '32'`

These facts can simply be used as variables in Ansible.

```yaml
- name: show the distribution
  debug:
    msg: "You are running {{ ansible_distribution }} version {{ ansible_distribution_major_version }}."
```

## [Handlers](#handlers)

Handlers are tasks that can be called when a parent tasks is changed. I know, it's a lot to take in but an example may help:

```yaml
- name: configure my servers
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: configure ntp
      template:
        src: ntp.conf.j2
        dest: /etc/ntp.conf
      notify:
        - restart ntp

  handlers:
    - name: restart ntp
      service:
        name: ntpd
        state: restarted
```

Here you see an (incomplete) example of how handlers can be used. The advantage of a handler is that many tasks from the `tasks` list can call handlers, it will only be executed once, at the end of the play or when something calls the [`meta`](https://docs.ansible.com/ansible/latest/modules/meta_module.html) module with `flush_handlers`.

## [when](#when)

You can run jobs [conditionally](https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html)  using `when`. It's like the `if` of bash.

```yaml
- name: install ntp on Fedora
  package:
    name: ntp
    state: present
  when:
    - ansible_distribution == "Fedora"
```

The `when` statement can handle a list of conditions, this is and "AND" list. To use "OR" simply write `or`:

```yaml
- name: install ntp on RedHat and Debian machines
  package:
    name: ntp 
    state: present
  when:
    - ansible_os_family == "RedHat" or ansible_os_family == "Debian"
```

## [loop](#loop)

You can repeat (most) tasks using a [`loop`](https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html) statement. This basically repeats the task and changed "{{ item }}" every run.

```yaml
- name: copy a file
  copy:
    src: "{{ item }}"
    dest: "/tmp/{{ item }}"
  loop:
    - my_file_1
    - my_file_2
```

## [Logic and data](#logic_and_data)

- Logic (code that determines how to configure a system) is best kept in [roles](#roles).
- Data (information that determines what to configure on a system) is best kept in [inventories](#inventories) and [host & group variables](#host_and_group_variables).

This allows a maintainer of a role to focus on the logic and the implementor to focus on settings that are correct for her/his environment.
