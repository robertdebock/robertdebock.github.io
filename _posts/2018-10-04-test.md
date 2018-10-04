---
layout: post
title: #AnsibleFest 2018
---

# AnsibleFest2018 (Austin, Texas)

So, it's been such a good week! [Dennis](https://github.com/velzend), Marco, [Jonathan](https://github.com/zinger) and [I](https://github.com/robertdebock) (and 1300 other Ansible fans) visited Ansible Fest 2018.

## Good to meet you
After working with quite some people online, I was really happy to finally meet some Ansible heroes:

- [Jeff Geerling](https://github.com/geerlingguy) - Ansible god, what a nice person.
- [David Moreau Simard](https://github.com/dmsimard) - Creator of [Ara](https://github.com/openstack/ara) and a real pleasure to speak to.
- [Chris Houseknecht](https://github.com/chouseknecht) - Galaxy contributor.
- [Victor da Costa](https://github.com/victorock) - Ansible network fan.
- [John Barker](https://github.com/gundalow) - Ansible community promotor.

## Highlights

Although nearly each talk was valuable, these are some (randomly ordered) takeaways that influence me:

- [Jeff Geerling](https://www.jeffgeerling.com/) recommends to include -Purpose-, -Links- and -Test instructions- t each `README.md` of a role.
- [Mazer](https://github.com/ansible/mazer) will be used to package role and send them off to [Galaxy](https://galaxy.ansible.com). It's not production ready now though.
- [Jeff Geerling](https://www.jeffgeerling.com/) recommends to `yamllint`, `--syntax-check` and `ansible-lint` on earch commit, also `molecule test` and `ansible-playbook --check` will improve quality.
- [Molecule](https://github.com/metacloud/molecule) will be much more integrated into the Ansible codebase.
- [Jeff Geerling](https://www.jeffgeerling.com/) recomments to use the callback plugins `profile_roles`, `profile_tasks` and `timer`, just as stdout_callback.
- [Galaxy](https://galaxy.ansible.com) will work on GitLab (and likely any Git provider) integration.
- [Galaxy](https://galaxy.ansible.com) will introduce code quality rating. Each role will be tested for [Ansible Lint rules](https://github.com/ansible/galaxy-lint-rules). Warnings and errors will reduce the amount of stars assigned. You can see this behaviour on the [Development Galaxy](https://galaxy-dev.ansible.com/robertdebock/bootstrap) for one of my roles.
- [Jeff Geerling](https://www.jeffgeerling.com/) recommends to use "flat" variables. Consider these two examples:

Difficult to overwrite a single value:
```yaml
apache:
  start_servers: 2
  max_clients: 2
```

Easy to overwrite:
```yaml
apache_start_servers: 2
apache_max_clients: 2
```

All in all, I'm really happy with the direction Ansible is going and feel that most decicions I've done in the past year are correct.

I expect that [molecule](https://github.com/metacloud/molecule) testing will be integrated into [Galaxy](https://galaxy.ansible.com) and reports will be created using [ARA](https://github.com/openstack/ara) will be integrated.
