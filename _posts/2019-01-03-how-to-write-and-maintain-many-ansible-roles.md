---
title: How to write and maintain many Ansible roles
---

# How to write and maintain many Ansible roles

It's great to have many code nuggets around to help you setup an environment rapidly. Ansible roles are perfect to describe what you want to do on systems.

As soon as you start to write more roles, you start to develop a style and way of working. Here are the tings I've learned managing many roles.

## Use a skeleton for stating a new role

When you start to write a new role, you can start with pre-populated code:

```sh
ansible-galaxy init --role-skeleton=ansible-role-skeleton role_name
```

To explain what happens:
- `ansible-galaxy` is a command. This may change to `molecule` in the future.
- `init` tells ansible-galaxy to initialize a new role.
- `--role-skeleton=ansible-role-skeleton` refers to a skeleton ansible role. I use [his repository](https://github.com/robertdebock/ansible-role-skeleton).
- `role_name` is the name of your new role. I use short names here, like `nginx` or `postfix`.

## Use ansible-lint for quick feedback

[Andrew](https://github.com/awcrosby) has written [a tool](https://github.com/ansible/ansible-lint) including [many rules](https://github.com/ansible/ansible-lint/tree/master/lib/ansiblelint/rules) that help you write readable and consistent code.

There are times where [I don't agree](https://github.com/ansible/ansible-lint/pull/409) to the rules, but the feedback is quickly processed.A

There are also times where I initially think rules are useless, but after a while [I'm convinced about the intent](https://robertdebock.nl/2018/11/01/lines-should-be-no-longer-than-120-chars.html) and change my code.

You can also [describe your preferences](https://github.com/robertdebock/ansible-lint-rules) and use ansible-lint to verify you code. Great for teams that need to agree on a style.

## Use molecule on Travis to test

In my opinion the most important part of writing code is testing. I spend a lot of time on writing and executing tests. It helps yourself to prove that certain scenarios work as intended.

Travis can help test your software. A typical commit takes some 30 to 45 minutes to test, but after that I know:

1. It works on the platforms I want to support.
2. When it works, the software is released to [Galaxy](https://galaxy.ansible.com/)
3. Pull requests are automatically tested.

It makes me less afraid of committing.

## Use versions or tags to release software

When I write some new functionality, I typically need a few iterations to make it work. Using GitHub releases helps me to capture (and release) a working version of a role.

You can play as much as you want in between releases, but when a release is done, the role should work.

### Go forth and develop!

You can setup a machine yourself for developing Ansible roles. I've [prepared a repository](https://github.com/robertdebock/ansible-development-environment) that may help.

The playbook in [that repository](https://github.com/robertdebock/ansible-development-environment) looks something like this:

```yaml
---
- name: setup an ansible development environment
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - robertdebock.bootstrap
    - robertdebock.update
    - robertdebock.fail2ban
    - robertdebock.openssh
    - robertdebock.digitalocean_agent
    - robertdebock.common
    - robertdebock.users
    - robertdebock.postfix
    - robertdebock.docker
    - robertdebock.investigate
    - robertdebock.ansible
    - robertdebock.ansible_lint
    - robertdebock.buildtools
    - robertdebock.molecule
    - robertdebock.ara
    - robertdebock.ruby
    - robertdebock.travis

  tasks:
    - name: copy private key
      copy:
        src: id_rsa
        dest: /home/robertdb/.ssh/id_rsa
        mode: "0400"
        owner: robertdb
        group: robertdb

    - name: copy git configuration
      copy:
        src: gitconfig
        dest: /home/robertdb/.gitconfig

    - name: create repository_destination
      file:
        path: "{{ repository_destination }}"
        state: directory
        owner: robertdb
        group: robertdb

    - name: clone all roles
      git:
        repo: "{{ repository_base }}/{{ item }}.git"
        dest: "{{ repository_destination }}/{{ item }}"
        accept_hostkey: yes
        key_file: /home/robertdb/.ssh/id_rsa
      with_items: "{{ repositories }}"
      become_user: robertdb
```
