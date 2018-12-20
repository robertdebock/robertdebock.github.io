---
title: When is a role a role
---

# When is a role a role

Sometimes it's not easy to see when Ansible code should be captured in an Ansible role, or when tasks can be used.

Here are some guidelines that help me decide when to choose for writing an Ansible role:

## Don't repeat yourself

When you start to see that your repeating blocks of code, it's probably time to move those tasks into an Ansible role.

Repeating yourself may:
- Introduce more errors
- Be more difficult to maintain

## Keep it simple

Over time Ansible roles tend to get more complex. [Jeff Geerling](https://www.jeffgeerling.com/) tries to [keep Ansible roles under 100 lines](https://www.ansible.com/blog/make-your-ansible-playbooks-flexible-maintainable-and-scalable). That can be a challenge, but I agree to Jeff.

Whenever I open up somebody else' Ansible role and the code keeps on scrolling, I tend to get demotivated:

- Where can you find the error/issue/bug?
- How can this be maintained?
- There is probaly no easy way to test this.
- The code does many things and misses focus.

## Cleanup your playbook

Another reason to put code in Ansible roles, is to keep your playbook easy to read. A long list of tasks is harder to read than a list of roles.

Take a look at this example:

```yaml
- name: build the backend server
  hosts: backend
  become: yes
  gather_facts: no

  roles:
    - robertdebock.bootstrap
    - robertdebock.update
    - robertdebock.common
    - robertdebock.python_pip
    - robertdebock.php
    - robertdebock.mysql
    - robertdebock.phpmyadmin
```

This code is simple to read, anybody could have an understanding what it does.

## When input is required

Some roles can have variables to change the installation, imagine this set of variables:

```yaml
httpd_port: 80
```

The role can assert variables, for example:

```yaml
- name: test input
  assert:
    that:
      - httpd_port <= 65535
      - httpd_port >= 1
```

## Check yourself

To verify that you've made the right decision:

1. Could you publish this role?

That means you did not put data in the role, except sane defaults.

2. Would anybody else be helped with your role?

That means you thought about the interface (`defaults/main.yml`).

3. Is there a simple way to test your role?

That means the role is focussed and can do just a few things.

4. Was it easy to think of the title?

That means you knew what you were building.

## Conclusion

Hope this helps you decide when a role is a role.
