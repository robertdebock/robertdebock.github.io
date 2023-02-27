---
title: Ansible lookup or conditionals
---

# Ansible lookup or conditionals

There are multiple ways to how Ansible deals with differences in distributions. Take these 2 examples:

## [Conditional](#conditional)

Here is an example that uses `when` to differentiate for different situations:

```yaml
- name: Install packages
  ansible.builtin.package:
    - name: python3-docker
  when:
    - ansible_os_family == "Debian"
```

The example above installs `python3-docker` if the managed node is "Debian". (`ansible_os_family` includes [Debian, Ubuntu and others](https://github.com/ansible/ansible/blob/devel/lib/ansible/module_utils/facts/system/distribution.py#L512).

## [Lookup](#lookup)

Another method is to make a map of distributions and define the required packages:

```yaml
_my_packages:
  Debian:
    - python3-docker

my_packages: "{% raw %}{{ _my_packages[ansible_os_family] | default([]) }}{% endraw %}"
```

Above you see the map `_my_packages`. It's basically a variable that is a dictionary or map. In this case the intent is to map different operating systems to a list of packages.

The `my_packages` variable looks up a value from the `_my_packages` map. The results in:

- `python3-docker` when the `ansible_os_family` is `Debian`.
- `[]` (an empty list) for any other `ansible_os_family`.

And just to complete the example: I prefer this method:

```yaml
# vars/main.yml
_my_packages:
  default: []
  Debian:
    - python3-docker

my_packages: "{% raw %}{{ _my_packages[ansible_os_family] | default('default') }}{% endraw %}"
```

```yaml
# tasks/main.yml
- name: Install packages
  ansible.builtin.package:
    - name: "{% raw %}{{ my_packages }}{% endraw %}"
```

As a maintainer of the above code, you can focus on the `_my_packages` map/dict. In my experience, this is simpler; Think of the logic once (`tasks/main.yml` or `playbook.yml`) and focus on "data" later. (`vars/main.yml` or anywhere else you'd put variables.)

By the way, I'm not sure how to call this mechanism, I guess "lookup" is pretty close.

## Conclusion

You can use either of the above examples; I prefer the [lookup](#lookup) method, but I see others using the [conditional](#conditional) method more often.

The best argument for me is ease of maintentance or readability.
