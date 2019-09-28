---
title: Ansible Fest Atlanta 2019
---

# Ansible Fest Atlanta 2019

Announcements on [Ansible](https://www.ansible.com/), [AWX](https://www.ansible.com/products/awx-project/faq), [Molecule](https://molecule.readthedocs.io/en/stable/), [Galaxy](https://galaxy.ansible.com/), [Ansible-lint](https://docs.ansible.com/ansible-lint/) and many other produts are always done on Ansible Fest.

Here is what I picked up on [Ansible Fest 2019 in Atlanta, Georgia](https://www.ansible.com/ansiblefest).

# Ansible Collections

Ansible if full of modules, ["batteries included"](https://www.ansible.com/use-cases/configuration-management) is a common expression. This reduces velocity in adding modules, fixing issues with modules or adding features to modules. Ansible Collections is there to solve this issue.

Ansible will (in a couple of releases) only be the framework, without modules or plugins. Modules will have to be installed seprarately.

There are a few unknowns:
- How to manage dependencies between collections and Ansible. For example, what collections work on which Ansible version.
- [The documentation of Ansible](https://docs.ansible.com/) is very important, but how to keep the same central point of documentation while spreading all these collections.
- How to deal with colliding module names? Imaging the `file` module is included in more than 1 collection, which one takes precedence?

Anyway, the big take-away: Start to learn to develop or use Ansible Collections, it's going to be important.

Here is how to [develop Ansible Collections](https://docs.ansible.com/ansible/devel/dev_guide/developing_collections.html) and how to [use them](https://docs.ansible.com/ansible/devel/user_guide/collections_using.html).

# AWX

[AWX](https://www.ansible.com/products/awx-project) is refactoring components to improve development velocity and the performance of the product itself.

- New UI, based on React and Pattern-Fly.
- `tower-cli` will be replaced by `awx`, which exposed the availabe commands based on the capabilities of the AWX API. The version of `awx` will be the same as the AWX web/api-tool.

# Data analysis

There are a few applications to analyse data and give insights on development and usage of Ansible:

- [Roles analysis](https://stats.eng.ansible.com/apps/galaxy/RolesStats.html)
- [Modules details](https://stats.eng.ansible.com/apps/docs/ModuleMomentum.html)
- [Pull requests per country](https://stats.eng.ansible.com/apps/docs/newmap.html)

There are many more perspectives, [have a look](https://stats.eng.ansible.com/apps/).

# Next Ansible Fest not in Europe

[Spain seems to be the largest contributor](https://stats.eng.ansible.com/apps/docs/newmap.html) of Ansible, but next Ansible Fest will be in San Diego.

The Contributers Summit will be in europe though.
