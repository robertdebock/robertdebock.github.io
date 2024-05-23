# [Tests](#tests)

The filosofy to test is:
- Test multiple distributions. (see `.gitlab-ci.yml` and `.github/workflows/molecule.yml`)

- Alpine
- Amazonlinux
- CentOS
- Debian
- Fedora
- OpenSuse
- Ubuntu

There are multiple tests configured, here is how they relate.

## [Unit tests](#unit-tests)

To test an Ansible role, molecule runs on a commit. This verifies that the role does it's job, but does not ensure that it works in combination with other roles.

### [Time based unit tests](#time-based-unit-tests)

Because distributions, molecule, and ansible change over time, a monthly test is done to all roles. The first letter of the role determines the day it's tested. For example `ansible-role-example` start with an `e`, runs on the 5th of the month.
