# Tests

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
