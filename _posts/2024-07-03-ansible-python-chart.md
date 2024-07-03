---
title: Ansible Python version chart
---

# Ansible Python version chart

Ansible ([ansible-core](https://pypi.org/project/ansible-core/)) depends on [Python](https://www.python.org/). Actually, the version of ansible-core has a strict requirements on the version of Python required.

Here is a table demonstrating the versions of ansible-core and their Python requirement.

## Overview

| anisble-core   | python         |
| -------------- | -------------- |
| 2.11.*         | >= 2.7, < 3.0  |
| 2.12.*, 2.13.* | >= 3.8         |
| 2.14.*, 2.15.* | >= 3.9         |
| 2.16.*, 2.17.* | >= 3.10        |

As you can see, Ansible version 2.12 was the first version of ansible-core to require python version 3.

## TOX

The tool "[tox](https://tox.wiki/en/3.4.0/config.html)" can be used to test multiple versions of pip modules in an exising Python installation. This is useful locally, to test if a certain python version and ansible version pass tests, or in CI to do these tests automatically.

Here is an example of a `tox.ini` configuration file.

```ini
[tox]

# Make a list of environments to test. This is a combination of python version and ansible version.
# Be aware, TOX does not install Python, the host that TOX runs on must have the required Python version(s) installed.
# A missing python version will result in a `SKIP`.
envlist =
    python2.7-ansible-2.11
    python3.8-ansible-2.12, python3.8-ansible-2.13
    python3.9-ansible-2.14, python3.9-ansible-2.15
    python3.10-ansible-2.16, python3.10-ansible-2.17

# These setting apply for all environments, listed further down.
[testenv]
commands = molecule test

# Some environment variables that are passed to the test environment.
# These variables are used by molecule, which allows you to run:
# $ image=ubuntu tag=latest molecule test.
# The DOCKER_HOST is set because I run docker on a remote host.
passenv =
    namespace
    image
    tag
    DOCKER_HOST

# Set some environment variables for the test environment.
# The TOX_ENVNAME is used to make molecule display a hostname, including the environment name.
setenv =
    TOX_ENVNAME={envname}

# The environments specified above, under the `envlist` section, should now define the Python version and the Ansible version.
[testenv:python2.7-ansible-2.11]
basepython = python2.7
deps =
    # Pickup the pip requirements from the requirements.txt file.
    -rrequirements.txt
    # Set a specific version of ansible-core.
    ansible-core == 2.11.*

[testenv:python3.8-ansible-2.12]
basepython = python3.8
deps =
    -rrequirements.txt
    ansible-core == 2.12.*

[testenv:python3.8-ansible-2.13]
basepython = python3.8
deps =
    -rrequirements.txt
    ansible-core == 2.13.*

[testenv:python3.9-ansible-2.14]
basepython = python3.9
deps =
    -rrequirements.txt
    ansible-core == 2.14.*

[testenv:python3.9-ansible-2.15]
basepython = python3.9
deps =
    -rrequirements.txt
    ansible-core == 2.15.*

[testenv:python3.10-ansible-2.16]
basepython = python3.10
deps =
    ansible-core == 2.16.*
    -rrequirements.txt

[testenv:python3.10-ansible-2.17]
basepython = python3.10
deps =
    ansible-core == 2.17.*
    -rrequirements.txt
```

The above configuration (`tox.ini`) works with a `requirements.txt`:

```text
molecule
molecule-plugins[docker]
ansible-lint
```

Good luck testing!
