# Tests

The filosofy to test is:
- Test multiple distributions
- Of each distribution, test the current and previous version
- Test 3 version of Ansible, current, previous and next previous.

In Travis CI these combinations are called a `matrix`. You can consider this overview per role:

| Distribution   | Ansible 2.3 | Ansible 2.4 | Ansible 2.5 |
|----------------|-------------|-------------|-------------|
| Alpine 3.6     | yes         | yes         | yes         |
| Alpine 3.7     | yes         | yes         | yes         |
| Archlinux      | yes         | yes         | yes         |
| Centos 6       | yes         | yes         | yes         |
| Centos 7       | yes         | yes         | yes         |
| Debian Buster  | yes         | yes         | yes         |
| Debian Jessie  | yes         | yes         | yes         |
| Debian Stretch | yes         | yes         | yes         |
| Fedora 26      | yes         | yes         | yes         |
| Fedora 27      | yes         | yes         | yes         |
| OpenSuse 42.2  | yes         | yes         | yes         |
| OpenSuse 42.3  | yes         | yes         | yes         |
| Ubuntu Artful  | yes         | yes         | yes         |
| Ubuntu Bionic  | yes         | yes         | yes         |

This matrix runs 14 (distributions) times 3 (Ansible versions) quals 42 build.

If a distribution or ansible version is not supported, the strategy is to also test that, and ensure if fails.

There are multiple tests configured, here is how they relate.

## Unit tests

To test an Ansible role, Travis CI runs molecule on a commit. This verifies that the role does it's job, but does not ensure that it works in combination with other roles.

An [example for the unit test for the Ansible role java](https://travis-ci.org/robertdebock/ansible-role-java).

## Integration

To test a combination of Ansible roles, Travis CI runs terraform and a complex playbook.

There is currently one [integration test](https://travis-ci.org/robertdebock/ansible-integration). A [report](https://robertdebock.nl/ansible-integration/) is saved every run.
