# [Tests](#tests)

The filosofy to test is:
- Test multiple distributions. (see `.gitlab-ci.yml` and `.github/workflows/molecule.yml`)
- Test multiple version of Ansible, previous, current and next previous and future. (see tox.ini)

| Distribution        | Ansible 2.9 | Ansible 2.10 | Ansible devel |
|---------------------|-------------|--------------|---------------|
| Alpine latest       | yes         | yes          | yes           |
| Alpine edge         | yes         | yes          | yes           |
| Archlinux (base)    | yes         | yes          | yes           |
| CentOS 7            | yes         | yes          | yes           |
| CentOS latest       | yes         | yes          | yes           |
| Debian stable       | yes         | yes          | yes           |
| Debian latest       | yes         | yes          | yes           |
| Debian unstable     | yes         | yes          | yes           |
| Fedora latest       | yes         | yes          | yes           |
| Fedora rawhide      | yes         | yes          | yes           |
| OpenSuse Leap       | yes         | yes          | yes           |
| OpenSuse Tumbleweed | yes         | yes          | yes           |
| Ubuntu Artful (17)  | yes         | yes          | yes           |
| Ubuntu latest       | yes         | yes          | yes           |
| Ubuntu devel        | yes         | yes          | yes           |

Read [this page to understand the tools (Travis, Molecule and Tox)](tox-molecule-travis.html) better.

There are multiple tests configured, here is how they relate.

## [Unit tests](#unit-tests)

To test an Ansible role, Travis CI runs molecule on a commit. This verifies that the role does it's job, but does not ensure that it works in combination with other roles.

### [Time based unit tests](#time-based-unit-tests)

Because distributions, molecule, and ansible change over time, a monthly test is done to all roles. The first letter of the role determines the day it's tested. For example `ansible-role-example` start with an `e`, runs on the 5th of the month.
