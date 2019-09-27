---
title: What I wish ansible collections would be
---

Ansible Collections is a way of:
1. Packaging Ansible Content (modules/roles/playbooks).
2. Distributing Ansible Content through Ansible Galaxy.
3. Reducing the size of Ansible Engine.

All modules that are now in Ansible will move to Ansible Collections.

I'm not 100% sure how Ansible Collections will work in the future, but here is a guess.

# From an Ansible role

I could imagine that requirements.yml will link depending modules **and** collections. Something like this:

```yaml
---
- src: robertdebock.x
  type: role
- src: robertdebock.y
  type: collection
```

That structure would ensure that all modules required to run the role are going to be installed.

# From an Ansible playbook repository

Identical to the role setup, I could imagine a `requirements.yml` that basically prepares the environment with all required dependencies, either roles or collections.

# Loop dependencies

Ansible Collections can depend on other Ansible Collections.

Imagine `my_collection`'s requirements.yml:

```yaml
---
- src: robertdebock.y
  type: collection
```

The Ansible Collection `y` could refer to `my_colletion`.

```
my_collection ---> y
       ^           |
       |           |
       +-----------+
```

I'm not sure how that can be resolved or prevented.
