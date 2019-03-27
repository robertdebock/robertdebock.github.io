---
title: Why should you use set_fact
---

# Why you should use the Ansible set_fact module

So far it seems that the Ansible `set_fact` module is not required very often. I found 2 cases in [the roles I write](https://robertdebock.nl/):

In the [awx role](https://galaxy.ansible.com/robertdebock/awx):

```yaml
- name: pick most recent tag
  set_fact:
    awx_version: "{{ item.name }}"
  with_items:
    - "{{ awx_get_tags.json | first }}"
```

In the [zabbix_server role](https://galaxy.ansible.com/robertdebock/zabbix_server):

```yaml
- name: find version of zabbix-server-mysql
  set_fact:
    zabbix_server_version: "{{ packages['zabbix-server-mysql'][0]['version'] }}"
```

In both cases a "complex" variable strucure is saved into a simpler to call variable name.

Variables that are constructed of other variables can be set in `vars/main.yml`. For example the [kernel](https://galaxy.ansible.com/robertdebock/kernel) role needs a version of the kernel in `defaults/main.yml`:

```yaml
kernel_version: 5.0.3
```

And the rest can be calculated in `vars/main.yml`:

```yaml
kernel_unarchive_src: https://cdn.kernel.org/pub/linux/kernel/v{{ kernel_version[:1] }}.x/linux-{{ kernel_version }}.tar.xz
```

So sometimes `set_fact` can be used to keep code simple, other (most) times `vars/main.yml` can help.

For a moral compass Southpark uses [Brian Boitano](https://www.youtube.com/watch?v=sNJmfuEWR8w), where my moral coding compass uses [Jeff Geerling](https://www.jeffgeerling.com/) who would say something like: "If your code is complex, it's probably not good."
