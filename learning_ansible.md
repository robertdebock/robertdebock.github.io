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
  pool.ntp.org
```

#### group_vars/amsterdam/ntp.yml

```yaml
ntp_servers:
  nl.pool.ntp.org
```

## [Playbooks](#playbooks)

### [Tasks](#tasks)

## [Roles](roles)
