---
title: Molecule and ARA
---

To test playbooks, [molecule](https://github.com/ansible/molecule) is really great. And since Ansible Fest 2018 (Austin, Texas) clearly communicated that Molecule will be a part of Ansible, I guess it's safe to say that [retr0h](https://github.com/retr0h)'s tool will be here to stay.

When testing, it's even nicer to have great reports. That's where [ARA](https://github.com/openstack/ara) comes in. ARA collects job output as a callback_plugin, saves it and is able to display it.

Here is how to set it up.

## Install molecule
```
pip install molecule
```

## Install ara
```
pip install ara
```

## Start ara
```
ara-manage runserver
```

## Configure molecule to use ara
Edit molecule.yml, under provisioner:
```
provisioner:
  name: ansible
  config_options:
    defaults:
      callback_plugins: /usr/lib/python2.7/site-packages/ara/plugins/callbacks
```

Now point your browser to http://localhost:9191/ and run a molecule test:
```
molecule test
```
