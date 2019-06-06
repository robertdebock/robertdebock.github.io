---
title: Fedora 30 and above use python-3
---

# Fedora 30 and above use python-3

Fedora 30 (and above) uses python 3 and start to deprecate python 2 package like `python2-dnf`.

Ansible 2.8 and above [discover the python interpreter](https://docs.ansible.com/ansible/latest/reference_appendices/interpreter_discovery.html), but Ansible 2.7 and lower do not have this feature.

So for a while, you have to tell Ansible to use python 3. This can be done by setting the `ansible_python_interpreter` somewhere. Here are a few locations you could use:

## 1. inventory

This is quite a good location, because you could decide to give a single node this variable:

inventory/host_vars/my_host.yml:

```yaml
---
ansible_python_interpreter: /usr/bin/python3
```

Or you could group hosts and apply a variable to it:

inventory/hosts:

```ini
[python3]
my_host
```

inventory/group_vars/python3.yml

```yaml
---
ansible_python_interpreter: /usr/bin/python3
````

## 2. extra vars

You could start a playbook and set the `ansible_python_interpreter` once:

```
ansible-playbook my_playbook.yml --extra_vars "ansible_python_interpreter=/usr/bin/python3"
```

It's not very persistent though.

## 3. playbook or role

You could save the variable in your playbook or role, but this makes re-using code more difficult; it will only work on machines with /usr/bin/python3:

```yaml
---
- name: do something
  hosts: all

  vars:
    ansible_python_interpreter: /usr/bin/python3

  tasks:
    - name: do some task
      debug:
        msg: "Yes, it works."
```

## 4. molecule

Last case I can think of it to let Molecule set `ansible_python_interpreter`.

molecule/default/molecule.yml:

```yaml
---
# Many parameters omitted.
provisioner:
  name: ansible
  inventory:
    group_vars:
      all:
        ansible_python_interpreter: /usr/bin/python3
# More parameters omitted.
```
