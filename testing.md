# Tests

The filosofy to test is:
- Test multiple distributions
- Of each distribution, test the current and previous version
- Test 3 version of Ansible, current, previous and next previous.

In Travis CI these combinations are called a `matrix`. You can consider this overview per role:

| Distribution       | Ansible 2.3 | Ansible 2.4 | Ansible 2.5 |
|--------------------|-------------|-------------|-------------|
| Alpine 3.6         | yes         | yes         | yes         |
| Alpine 3.7         | yes         | yes         | yes         |
| Archlinux (base)   | yes         | yes         | yes         |
| Centos 6           | yes         | yes         | yes         |
| Centos 7           | yes         | yes         | yes         |
| Debian Buster (10) | yes         | yes         | yes         |
| Debian Stretch (9) | yes         | yes         | yes         |
| Fedora 26          | yes         | yes         | yes         |
| Fedora 27          | yes         | yes         | yes         |
| OpenSuse 42.2      | yes         | yes         | yes         |
| OpenSuse 42.3      | yes         | yes         | yes         |
| Ubuntu Artful (17) | yes         | yes         | yes         |
| Ubuntu Bionic (18) | yes         | yes         | yes         |

This matrix runs 13 (distributions) times 3 (Ansible versions) equals 39 build.

If a distribution or ansible version is not supported, the strategy is to also test that, and ensure if fails.

There are multiple tests configured, here is how they relate.

## Unit tests

To test an Ansible role, Travis CI runs molecule on a commit. This verifies that the role does it's job, but does not ensure that it works in combination with other roles.

An [example for the unit test for the Ansible role java](https://travis-ci.org/robertdebock/ansible-role-java).

### Time based unit tests

Because distriutions, molecule, ansible and goss change over time, a monthly test is done to all roles using this schedule:

|------------|------|------------|
|Day of month|Letter|Ansible Role|
|1|a|[ansible-role-ara](https://travis-ci.org/robertdebock/ansible-role-ara)|
|2|b|[ansible-role-bootstrap](https://travis-ci.org/robertdebock/ansible-role-bootstrap)|
|2|b|[ansible-role-buildtools](https://travis-ci.org/robertdebock/ansible-role-buildtools)|
|3|c|[ansible-role-clamav](https://travis-ci.org/robertdebock/ansible-role-clamav)|
|4|d|[ansible-role-dhcpd](https://travis-ci.org/robertdebock/ansible-role-dhcpd)|
|4|d|[ansible-role-dns](https://travis-ci.org/robertdebock/ansible-role-dns)|
|4|d|[ansible-role-docker](https://travis-ci.org/robertdebock/ansible-role-docker)|
|4|d|[ansible-role-dovecot](https://travis-ci.org/robertdebock/ansible-role-dovecot)|
|5|e|[ansible-role-epel](https://travis-ci.org/robertdebock/ansible-role-epel)|
|6|f|[ansible-role-fail2ban](https://travis-ci.org/robertdebock/ansible-role-fail2ban)|
|8|h|[ansible-role-haveged](https://travis-ci.org/robertdebock/ansible-role-haveged)|
|9|i|[ansible-role-httpd](https://travis-ci.org/robertdebock/ansible-role-httpd)|
|10|j|[ansible-role-iptables](https://travis-ci.org/robertdebock/ansible-role-iptables)|
|11|k|[ansible-role-java](https://travis-ci.org/robertdebock/ansible-role-java)|
|13|m|[ansible-role-mssql](https://travis-ci.org/robertdebock/ansible-role-mssql)|
|13|m|[ansible-role-mysql](https://travis-ci.org/robertdebock/ansible-role-mysql)|
|14|n|[ansible-role-natrouter](https://travis-ci.org/robertdebock/ansible-role-natrouter)|
|14|n|[ansible-role-nginx](https://travis-ci.org/robertdebock/ansible-role-nginx)|
|14|n|[ansible-role-npm](https://travis-ci.org/robertdebock/ansible-role-npm)|
|14|n|[ansible-role-ntp](https://travis-ci.org/robertdebock/ansible-role-ntp)|
|16|p|[ansible-role-php](https://travis-ci.org/robertdebock/ansible-role-php)|
|16|p|[ansible-role-phpmyadmin](https://travis-ci.org/robertdebock/ansible-role-phpmyadmin)|
|16|p|[ansible-role-postfix](https://travis-ci.org/robertdebock/ansible-role-postfix)|
|16|p|[ansible-role-python-pip](https://travis-ci.org/robertdebock/ansible-role-python-pip)|
|18|r|[ansible-role-release](https://travis-ci.org/robertdebock/ansible-role-release)|
|18|r|[ansible-role-revealmd](https://travis-ci.org/robertdebock/ansible-role-revealmd)|
|18|r|[ansible-role-roundcubemail](https://travis-ci.org/robertdebock/ansible-role-roundcubemail)|
|18|r|[ansible-role-rsyslog](https://travis-ci.org/robertdebock/ansible-role-rsyslog)|
|18|r|[ansible-role-ruby](https://travis-ci.org/robertdebock/ansible-role-ruby)|
|18|r|[ansible-role-rundeck](https://travis-ci.org/robertdebock/ansible-role-rundeck)|
|19|s|[ansible-role-scl](https://travis-ci.org/robertdebock/ansible-role-scl)|
|19|s|[ansible-role-spamassassin](https://travis-ci.org/robertdebock/ansible-role-spamassassin)|
|20|t|[ansible-role-tftpd](https://travis-ci.org/robertdebock/ansible-role-tftpd)|
|20|t|[ansible-role-tomcat](https://travis-ci.org/robertdebock/ansible-role-tomcat)|
|21|u|[ansible-role-update](https://travis-ci.org/robertdebock/ansible-role-update)|
|24|x|[ansible-role-xinetd](https://travis-ci.org/robertdebock/ansible-role-xinetd)|
|26|z|[ansible-role-zabbix](https://travis-ci.org/robertdebock/ansible-role-zabbix)|

## Integration

To test a combination of Ansible roles, Travis CI runs terraform and a complex playbook.

There is currently one [integration test](https://travis-ci.org/robertdebock/ansible-integration). A [report](https://robertdebock.nl/ansible-integration/) is saved every run.
