---
title: Galaxy will score your roles
---

# Ansible Galaxy Lint

Galaxy currently is a dumping place for Ansible roles, anybody can submit any quality role there and it's kept indefinitely.

For example [Nginx is listed 1122 times](https://galaxy.ansible.com/search?keywords=nginx&order_by=-relevance&page_size=10). Happily [Jeff Geerling](https://github.com/geerlingguy)'s role shows up on top, probably because it has the most downloads.

The [Galaxy team](https://github.com/ansible/galaxy/graphs/contributors) has decided that checking for quality is one way to improve search results. It looks liek roles will have a few criterea:

- Number of downloads.
- Score (using [Ansible Lint](https://github.com/willthames/ansible-lint))
- Feedback

The rules are stored in [galaxy-lint-roles](https://github.com/ansible/galaxy-lint-rules). So far [Andrew](https://github.com/awcrosby), [House](https://github.com/chouseknecht) and [Robert](https://github.com/robertdebock) have contributed, feel free to propose new rules or improvements!

You can prepare your roles:
```
cd directory/to/save/the/rules
git clone https://github.com/ansible/galaxy-lint-rules.git
cd directory/to/your/role
ansible-lint -r directory/to/save/the/rules/galaxy-lint-rules/rules .
```

I've removed quite a few errors by using these rules:
- Missing spaces after {% raw %}{{{% endraw %} or before {% raw %}}}{% endraw %}.
- Comparing booleans using `== yes`.
- meta/main.yml mistakes

You can peek how your roles are scored on [development](https://galaxy-dev.ansible.com).
