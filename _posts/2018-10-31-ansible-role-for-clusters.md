---
title: Ansible roles for clusters
---

Ansible can be used to configure clusters. It's actually quite easy!

Typically a cluster has some master/primary/active node, where stuff needs to be done and other stuff needs to be done on the rest of the nodes.

Ansible can use `run_once: yes` on a task, which "automatically" selects a primary node. Take this example:

inventory:
```
host1
host2
host3
```

tasks/main.yml:
```
- name: do something on all nodes
  package:
    name: screen
    state: present

- name: select the master/primary/active node
  set_fact:
    master: "{{ inventory_hostname }}"
  run_once: yes

- name: do something to the master only
  command: id
  when:
    - inventory_hostname == master

- name: do something on the rest of the nodes
  command: id
  when:
    - inventory_hostname != master
```

It's a simple and understandable solution. You can even tell Ansible that you would like to pin a master:
```
- name: select the master/primary/active node
  set_fact:
    master: "{{ inventory_hostname }}"
  run_once: yes
  when:
    - master is not defined
```

In the example above, if you set "master" somewhere, a user can choose to set a master instead of "random" selection.

Hope it helps you!
