---
title: Ansible testing compnents
---

# Ansible testing compnents

To test Ansible, I use quite some components. This page lists the components uses, their versions, and where they are used.

|Component             |Used   |Latest |Used where                            |
|----------------------|-------|-------|--------------------------------------|
|ansible               |2.9    |2.9.18 |tox.ini                               |
|ansible               |2.10   |2.10.7 |tox.ini                               |
|molecule              |>=3,<4 |![molecule][molecule]|docker-github-action-molecule         |
|tox                   |latest |n.a.   |docker-github-action-molecule         |
|ansible-lint          |latest |v5.0.2 |docker-github-action-molecule         |
|pre-commit            |2.9.3  |v2.10.1|nstalled on development desktop.     |
|molecule-action       |2.6.16 |2.6.16 |.github/workflows/molecule.yml        |
|github-action-molecule|3.0.6  |3.0.6  |.gitlab-ci.yml                        |
|ubuntu                |20.04  |20.04  |.github/workflows/galaxy.yml          |
|ubuntu                |20.04  |20.04  |.github/workflows/molecule.yml        |
|ubuntu                |20.04  |20.04  |.github/workflows/requirements2png.yml|
|ubuntu                |20.04  |20.04  |.github/workflows/todo.yml            |
|galaxy-action         |1.1.0  |1.1.0  |.github/workflows/galaxy.yml          |
|graphviz-action       |1.0.7  |1.0.7  |.github/workflows/requirements2png.yml|
|checkout              |v2     |v2.3.4 |.github/workflows/requirements2png.yml|
|checkout              |v2     |v2.3.4 |.github/workflows/molecule.yml        |
|todo-to-issue         |v2.3   |v2.4.1 |.github/workdlows/todo.yml            |
|python                |3.9    |3.9    |.travis.yml                           |
|pre-commit-hooks      |v3.4.0 |v3.4.0 |.pre-commit-config.yaml               |
|yamllint              |v1.26.0|v1.26.0|.pre-commit-config.yaml               |
|pre-commit            |v1.1.2 |v2.10.1|.pre-commit-config.yaml               |
|fedora                |33     |33     |docker-github-action-molecule         |

[molecule]: https://img.shields.io/pypi/v/molecule "Latest Molecule vrsion."
