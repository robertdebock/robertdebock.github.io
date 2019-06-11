---
title: Anisble Galaxy Collections are here!
---

# Ansible Galaxy Collections are here!

As [the documentation](https://galaxy.ansible.com/docs/mazer/examples.html) describes:

```
Collections are a new way to package and distribute Ansible related content.
```

I write [a lot of roles](https://robertdebock.nl/), roles are nice, but it's a bit like ingredients without a recipe: A role is only a part of the whole picture.

Collections allow you to package:
- roles
- actions
- filters
- lookup plugins
- modules
- strategies

So instead of [upstreaming](https://en.wikipedia.org/wiki/Upstream_(software_development) content to Ansible, you can publish or consume content yourself.

The whole process is [documented](https://galaxy.ansible.com/docs/contributing/creating_collections.html) and should not be difficult.

I've published my [development_environment](https://github.com/robertdebock/ansible-development-environment) and only had to change these things:

1. Add `galaxy.yml`

```
namespace: "robertdebock"
name: "development_environment"
description: Install everything you need to develop Ansible roles.
version: "1.0.4"
readme: "README.md"
authors:
    - "Robert de Bock"
dependencies:
license:
    - "Apache-2.0"
tags:
    - development
    - molecule
    - ara
repository: "https://github.com/robertdebock/ansible-development-environment"
documentation: "https://github.com/robertdebock/ansible-development-environment/blob/master/README.md"
homepage: "https://robertdebock.nl"
issues: "https://github.com/robertdebock/ansible-development-environment/issues"
```

2. Enable Travis for the repository

Go to [Travis](https://travis-ci.org/account/repositories) and click `Sync account`. Wait a minute or so and enable the repository containing your collection.

3. Save a hidden variable in Travis

Under `settings` for a repository you can find `Environment Variables`. Add one, I called it `galaxy_api_key`. You'll refer to this variable in `.travis.yml` later.

4. Add `.travis.yml`

```yaml
---
language: python

install:
  - pip install mazer
  - release=$(mazer build | tail -n1 | awk '{print $NF}')

script:
  - mazer publish --api-key=${galaxy_api_key} ${release}
```

Bonus hint: Normally you don't save roles, so you add something like `roles/*` to `.gitignore`, but in this case it is a part of the collection. So if you have `requirements.yml`, download all the roles locally using `ansible-galaxy install -r roles/requirements.yml -f` and include them in the commit.
