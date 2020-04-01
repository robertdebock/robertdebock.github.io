---
title: Debugging services in Ansible
---

# Debugging services in Ansible

Sometimes services don't start and give an error like:

```
Unable to start service my_service: A dependency job for my_service.service failed. See 'journalctl -xe' for details.
```

Well, if you're testing in CI, you can't really access the instance that has an issue. So how to you troubleshoot this?

I use this pattern frequently:

```yaml
- name: debug start my_service
  block:
    - name: start my_service
      service:
        name: "{{ my_service_service }}"
        state: started
  rescue:
    - name: collect information
      command: "{{ item }}"
      register: my_service_collect_information
      loop:
        - journalxtl -xe
        - systemctl status {{ my_service_service }}
    - name: show information
      debug:
        msg: "{{ item }}"
      loop: "{{ my_service_collect_information.results }}"
```

What's happening here?

- The `block` is a grouping of tasks.
- The `rescue` runs when the initial task (`start my_service`) fails. It has two tasks.
- The task `collect information` runs a few commands, add extra is you know more and saves the results in `my_service_collect_information`.
- The task `show information` displays the saved result. Because `collect information` has a loop, the variable has `.results` appended, which is a list to needs to be looped over.

Hope this helps you troubleshoot services in Travis of Google Actions.
