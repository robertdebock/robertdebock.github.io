---
title: Ansible alternatives for shell tricks
---

# Ansible alternatives for shell tricks

If you're used to shells and their commands like bash, sed  and grep, here are a few alternatives for Ansible.

Using these *native* alternatives has an advantage, developers maintain the Ansible modules for you, they are idempotent and likely work on more distributions or platforms.

## Grep

Imagine you need to know if a certain patter is in a file. With a shell script you would use something like this:

```bash
grep "pattern" file.txt && do-something-if-pattern-is-found.sh
```

With Ansible you can achieve a similar result like this:

```yaml
--- 
- hosts: all
  gather_facts: no

  tasks:
    - name: check if pattern if found
      lineinfile:
        path: file.txt
        regexp: '.*pattern.*'
        line: 'whatever'
      register: check_if_pattern_is_found
      check_mode: yes
      notify: do something

  handlers:
    - name: do something
      command: do-something-if-pattern-is-found.sh
```

Hm, much longer than the bash example, but native Ansible!

## Sed

So you like `sed`? So do I. It's one of the most powerful tools I've used.

Lets replace some pattern in a file:

```bash
sed 's/pattern_a/pattern_b/g' file.txt
```

This would repace all occurences of `pattern_a` for `pattern_b`. Lets see what Ansible looks like.

```yaml
---
- name: replace something
  gather_facts: no

  tasks:
    - name: replace patterns
      lineinfile:
        path: file.txt
        regexp: '^(.*)pattern_a(.*)$'
        line: '\1pattern_b\2'
```

Have a look at the [lineinfile module documentation](https://docs.ansible.com/ansible/latest/modules/lineinfile_module.html) for more details.

## Find and remove.

The `find` (UNIX) tools is really powerful too, imagine this command:

```bash
find / -name some_file.txt -exec rm {} \;
```

That command would find all files (and directories) named some_file.txt and remove them. A bit dangerous, but powerful.

With Ansible:

```yaml
- name: find and remove
  gather_facts: no

  tasks:
    - name: find files
      find:
        paths: /
        patterns: some_file.txt
      register: found_files

    - name: remove files
      file:
        path: "{% raw %}{{ item.path }}{% endraw %}"
        state: absent
      loop: "{% raw %}{{ found_files.results }}{% endraw %}"
```

## Conclusion

Well, have fun with all non-shell solutions. You hardly needs the `shell` or `command` modules when you get the hang of it.
