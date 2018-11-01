---
title: [204] Lines should be no longer than 120 chars
---

# [204] Lines should be no longer than 120 chars

It seems [Galaxy](https://galaxy.ansible.com) is going to use [galaxy-lint-rules](https://github.com/ansible/galaxy-lint-rules) to star roles.
One of the controls tests the length of the lines. Here are a few way so pass those rules.

## Spread over lines
In YAML you can use [multi line[(https://yaml-multiline.info/) to spread long lines.

### Without new lines
The `>` character replaces newlines by spaces.
```
- name: demostrate something
  debug: 
    msg: >
      This will just
      be a single long
      line.
```

### With new lines
The `|` character keeps newlines.
```
- name: demostrate something
  debug:
    msg: |
      The following lines
      will be spread over
      multiple lines.
```

## Move long lines to vars
Sometimes variables can get very long. You can save a longer variable in a shorter one.

For example, too long would be this task in main.yml:
```
- name: unarchive zabbix schema
  command: gunzip /usr/share/doc/zabbix-server-{{ zabbix_server_type }}-{{ zabbix_version_major }}.{{ zabbix_version_minor }}/create.sql.gz
```

Copy-paste that command to vars/main.yml:
```
gunzip_command: "gunzip /usr/share/doc/zabbix-server-{{ zabbix_server_type }}-{{ zabbix_version_major }}.{{ zabbix_version_minor }}/create.sql.gz"
```

And change main.yml to simply:
```
- name: unarchive zabbix schema
  command: "{{ gunzip_command }}"
```

## Conclusion
Yes it's annoying to have a limitation like this, but it does make the code more readable and it's not difficult to change your roles to get 5 stars.
