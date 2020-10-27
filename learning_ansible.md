# [Learning Ansible](#learning-ansible)

This is an as-short-as-possible walk through to learn Ansible. It should help you get going as quick as possible.

## Concept

[Ansible](https://www.ansible.com/) is a tool that can configure (remote) servers. It's simple to read, has many [modules](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html) and can run from a simple laptop.

With Ansible you describe what [tasks](#tasks) should be executed on selected remote hosts in the [inventory](#inventory).

```text
+--- command ------------------+
| ansible-playbook my_play.yml |
+------------------------------+
   |
   V
+--- ansible.cfg -----+   +--- my_play.yml ------------------+
| inventory=inventory |   | - name: configure my hosts       |
+---------------------+   |   hosts: all                     |
   |                      |   become: yes                    |
   V                      |   gather_facts: yes              |
+--- inventory ---+       |                                  | 
| my_host_1       |-----> |   tasks:                         |
| my_host_2       |       |     - name: show ntp_servers     |
+-----------------+       |       debug:                     |
                          |         msg: "{{ ntp_servers }}" | 
                          +----------------------------------+
                             ^                           |
                             |                           V
+--- group_vars/all/ntp.yml ---+   +--- my_host_1 ----------+
| ntp_servers:                 |   | ntp_servers:           |
|   - name: pool.ntp.org       |   |   - name: pool.ntp.org |
+------------------------------+   +------------------------+
```

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

## [Roles](#roles)

When you are writing more [playbooks](#playbooks), you'll see that duplicate code creeps in. You may have 2 playbooks that both configure and start ntp.

That's a perfect opportunity to write a `role`. A role is basically a list of tasks that you can pull into a play:

```yaml
---
- name: configure my servers
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.ntp
```

The role itself has a few files and directories. This is a simplified version of my [ntp role](https://github.com/robertdebock/ansible-role-ntp).

```
.
├── defaults
│   └── main.yml      <- This contains user-overwritable settings, such as `ntp_servers`.
├── files
│   └── ntpd          <- Files that are copied to the remote system.
├── handlers
│   └── main.yml      <- Handlers are described further below.
├── meta
│   └── main.yml      <- Information for Ansible Galaxy, where Ansible roles can be published.
├── README.md         <- Documentation, give this the right amount of attention.
├── requirements.yml  <- Depending roles can be downloaded if specified here.
├── tasks
│   └── main.yml      <- The logic of the role, a list of tasks.
├── templates
│   └── ntp.conf.j2   <- Files that are rendered and placed on the remote system.
└── vars
    └── main.yml      <- Variables that are not supposed to be overwritten like `ntp_packages`.
```


### [defaults/main.yml](#defaults_main_yml)

This file contains variables that a user/[playbook](#playbook) can easily overwrite. Typically this contains settings for a program or role. You need to choose "sane defaults" so that a user that does not overwrite values still has a working application.

```yaml
---
ntp_servers:
  - name: pool.ntp.org
```

You can use comments here to guide a user of your role on how to use these variables.

## [files/*](#files)

Files in here can be copied to a remote machine. The copying itself needs to be described in [`tasks/main.yml`](#tasks_main_yml).

### [handers/main.yml](#handlers_main_yml)

Handlers contain named tasks that are only started when they are called. I know, it's a lot to take in. Here is an example:

tasks/main.yml

```yaml
- name: configure ntp
  template:
    src: ntp.conf.j2
    dest: /etc/ntp.conf
  notify:
    - restart ntp
```

handlers/main.yml

```yaml
---
- name: restart ntp
  service:
    name: ntp
    state: restarted
```

So, only if the task `configure ntp` is `changed`, the handler `restart ntp `is called. You can run roles over and over again, only the times where `ntp.conf` is changed, ntp is restarted. Quite logical right?

Handlers run at:
1. The end of a play.
2. When `meta: flush_handlers` runs.

The order of handers is as they are ordered in the `handlers/main.yml`, NOT how they are called.

### [meta/main.yml](#meta_main_yml)

This file is used to tell [Ansible Galaxy](https://galaxy.ansible.com/) how to show your role. Some examples of the contens:

```yaml
galaxy_info:
  author: Robert de Bock
  role_name: ntp
  description: Install and configure ntp on your system.
  license: Apache-2.0
  company: none
  min_ansible_version: 2.8

  platforms:
    - name: Fedora
      versions:
        - 32
        - 33

  galaxy_tags:
    - ntp

dependencies: []
```

### [README.md](#readme_md)

### [requirements.yml](#requirements_yml)

### [tasks/main.yml](#tasks_main_yml)

### [templates/*.j2](#templates_j2)

### [vars/main.yml](#vars_main_yml)

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
    msg: "You are running {% raw %}{{ ansible_distribution }}{% endraw %} version {% raw %}{{ ansible_distribution_major_version }}{% endraw %}
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

You can repeat (most) tasks using a [`loop`](https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html) statement. This basically repeats the task and changed "{% raw %}{{ item }}{% endraw %} every run.

```yaml
- name: copy a file
  copy:
    src: "{% raw %}{{ item }}{% endraw %}
    dest: "/tmp/{% raw %}{{ item }}{% endraw %}
  loop:
    - my_file_1
    - my_file_2
```

## [Logic and data](#logic_and_data)

- Logic (code that determines how to configure a system) is best kept in [roles](#roles).
- Data (information that determines what to configure on a system) is best kept in [inventories](#inventories) and [host & group variables](#host_and_group_variables).

This allows a maintainer of a role to focus on the logic and the implementor to focus on settings that are correct for her/his environment.
