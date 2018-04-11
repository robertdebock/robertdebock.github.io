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
| Centos 6       | yes         | yes         | yes         |
| Debian Jessie  | yes         | yes         | yes         |
| Debian Stretch | yes         | yes         | yes         |
| Fedora 26      | yes         | yes         | yes         |
| Fedora 27      | yes         | yes         | yes         |
| OpenSuse 42.2  | yes         | yes         | yes         |
| OpenSuse 42.3  | yes         | yes         | yes         |

This matrix runs 11 (distributions) times 3 (Ansible versions) equals 33 build.

If a distribution of ansible version is not supported, the strategy is to also test that, but ensure if fails.

There are multiple tests configured, here is how they relate

## Unit tests

To test an Ansible role, Travis CI run molecule on a commit.

## Integration

To test a combination of Ansible roles, Travis CI runs terraform and a complex playbook.

## Travis CI

```
script:
# This is the unit test
  - molecule test
# This is the integration test
  - if [ "$TRAVIS_BRANCH" = "master" ]; then \
      terraform apply
      sleep 30
      ansible-playbook -i integration integration.yml
    fi
 
after_script
# Always (success or failure) clean up the machines
  - terraform destroy

notifications:
# On success, publish the role to Ansible Galaxy
  webhooks: https://galaxy.ansible.com/api/v1/notifications/
```
