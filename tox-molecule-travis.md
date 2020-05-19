# [Tox, Molecule and Travis](#tox-molecule-and-travis)

Testing uses a few tools to reduce the amount of code/lines and thus reduces the amount of errors. Because this increases complexity, this page described how the three work together.

# [Molecule](#molecule)

[Molecule](https://molecule.readthedocs.io/en/stable/) is used to test an Ansible role. Molecule know "scenarios". I use one called `default`. That default scenario optionally listens to an environment variable to determine what distribution to test: `distribution`.

This is how you can use it:

```
export distribution="fedora"
molecule test
```

This `distribution` variable refers to a container image hosted on [Docker Hub](https://hub.docker.com/), so it can also be `fedora:latest` for example.

# [Travis](#travis)

[Travis](https://travis-ci.org/) uses "matrix" to feed variable into the builds. For example:

```
env:
  matrix:
    - distribution="fedora"
    - distribution="centos"
    - distribution="ubuntu"
```

This means that Travis will run 3 times, setting the variable distribution to `fedora`, `centos` and finally `ubuntu`.

# [Tox](#tox)

The final piece of the puzzle is [tox](https://tox.readthedocs.io/en/latest/). This is a tool that sets up the Python virtualenv with the required pip packages.

For example this piece of tox.ini tells tox to setup three virtualenvs:

```
# This is not all the contents of tox.ini, just an example
[tox]
envlist = py{37}-ansible-{previous,current,next}

[testenv]
deps =
    previous: ansible~=2.7
    current: ansible~=2.8
    next: git+https://github.com/ansible/ansible.git@devel
    docker
    molecule
commands =
    molecule test
```

# ["Graphical" overview](#graphical-overview)

```
            +---------------------+   +---------------------+   +---------------------+
travis ---> | distribution=fedora |   | distribution=centos |   | distribution=ubuntu |
tox ------> |   - anible-2.7      |   |   - ansible-2.7     |   |   - ansible-2.7     |
tox ------> |   - anible-2.8      |   |   - ansible-2.8     |   |   - ansible-2.8     |
tox ------> |   - anible-devel    |   |   - ansible-devel   |   |   - ansible-devel   |
            | commands:           |   | commands:           |   | commands:           |
molecule -> |   - molecule test   |   |   - molecule test   |   |   - molecule test   |
            +---------------------+   +---------------------+   +---------------------+
```
