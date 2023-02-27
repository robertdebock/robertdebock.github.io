---
title: Ansible lookup or conditionals
---

# Ansible lookup or conditionals

There are multiple ways to how Ansible deals with differences in distributions. Take these 2 examples:

## Conditional

Here is an example that uses `when` to differentiate for different situations:

```yaml
- name: Install packages
  ansible.builtin.package:
    - name: python3-docker
  when:
    - ansible_os_family == "Debian"
```

The example above installs `python3-docker` if the managed node is "Debian". (`ansible_os_family` includes [Debian, Ubuntu and others](https://github.com/ansible/ansible/blob/devel/lib/ansible/module_utils/facts/system/distribution.py#L512).

## Lookup

Another method is to make a map of distributions and define the required packages:

```yaml
_my_packages:
  Debian:
    - python3-docker

my_packages: "{{ _my_packages[ansible_os_family] | default([]) }}"
```

Above you see the map `_my_packages`. It's basically a variable that is a dictionary or map. In this case the intent is to map different operating systems to a list of packages.

The `my_packages` variable looks up a value from the `_my_packages` map. The results in:

- `python3-docker` when the `ansible_os_family` is `Debian`.
- `[]` (an empty list) for any other `ansible_os_family`.

And just to complete the example: I prefer this method:

```yaml
_my_packages:
  default: []
  Debian:
    - python3-docker

my_packages: "{{ _my_packages[ansible_os_family] | default('default') }}"
```

As a maintainer of the above code, you can focus on the `_my_packages` map/dict. In my experience, this is simpler; Think of the logic once (`tasks/main.yml` or `playbook.yml`) and focus on "data" later. (`vars/main.yml` or anywhere else you'd put variables.)

## Conclusion

You can use either of the above examples; I prefer the "lookup" method.
