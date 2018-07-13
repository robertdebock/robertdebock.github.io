# Tests

The filosofy to test is:
- Test multiple distributions
- Of each distribution, test the current and previous version
- Test 3 version of Ansible, current, previous and next previous.

In Travis CI these combinations are called a `matrix`. You can consider this overview per role:

| Distribution        | Ansible 2.3 | Ansible 2.4 | Ansible 2.5 |
|---------------------|-------------|-------------|-------------|
| Alpine latest       | yes         | yes         | yes         |
| Alpine edge         | yes         | yes         | yes         |
| Archlinux (base)    | yes         | yes         | yes         |
| Centos 6            | yes         | yes         | yes         |
| Centos latest       | yes         | yes         | yes         |
| Debian stable       | yes         | yes         | yes         |
| Debian latest       | yes         | yes         | yes         |
| Fedora latest       | yes         | yes         | yes         |
| Fedora rawhide      | yes         | yes         | yes         |
| OpenSuse Leap       | yes         | yes         | yes         |
| OpenSuse Tumbleweed | yes         | yes         | yes         |
| Ubuntu Artful (17)  | yes         | yes         | yes         |
| Ubuntu latest       | yes         | yes         | yes         |

This matrix runs 13 (distributions) times 3 (Ansible versions) equals 39 build.

If a distribution or ansible version is not supported, the strategy is to also test that, and ensure if fails.

There are multiple tests configured, here is how they relate.

## Unit tests

To test an Ansible role, Travis CI runs molecule on a commit. This verifies that the role does it's job, but does not ensure that it works in combination with other roles.

An [example for the unit test for the Ansible role java](https://travis-ci.org/robertdebock/ansible-role-java).

### Time based unit tests

Because distriutions, molecule, ansible and goss change over time, a monthly test is done to all roles using this schedule:

|------------|------------|
|Day of month|Ansible Role|
|1|[ansible-role-at](https://travis-ci.org/robertdebock/ansible-role-at)|
|1|[ansible-role-ara](https://travis-ci.org/robertdebock/ansible-role-ara)|
|2|[ansible-role-bootstrap](https://travis-ci.org/robertdebock/ansible-role-bootstrap)|
|2|[ansible-role-buildtools](https://travis-ci.org/robertdebock/ansible-role-buildtools)|
|3|[ansible-role-cargo](https://travis-ci.org/robertdebock/ansible-role-cargo)|
|3|[ansible-role-clamav](https://travis-ci.org/robertdebock/ansible-role-clamav)|
|4|[ansible-role-dhcpd](https://travis-ci.org/robertdebock/ansible-role-dhcpd)|
|4|[ansible-role-dns](https://travis-ci.org/robertdebock/ansible-role-dns)|
|4|[ansible-role-docker](https://travis-ci.org/robertdebock/ansible-role-docker)|
|4|[ansible-role-dovecot](https://travis-ci.org/robertdebock/ansible-role-dovecot)|
|5|[ansible-role-epel](https://travis-ci.org/robertdebock/ansible-role-epel)|
|6|[ansible-role-fail2ban](https://travis-ci.org/robertdebock/ansible-role-fail2ban)|
|7|[ansible-role-gotop](https://travis-ci.org/robertdebock/ansible-role-gotop)|
|8|[ansible-role-haveged](https://travis-ci.org/robertdebock/ansible-role-haveged)|
|8|[ansible-role-httpd](https://travis-ci.org/robertdebock/ansible-role-httpd)|
|9|[ansible-role-iptables](https://travis-ci.org/robertdebock/ansible-role-iptables)|
|10|[ansible-role-java](https://travis-ci.org/robertdebock/ansible-role-java)|
|13|[ansible-role-mitogen](https://travis-ci.org/robertdebock/ansible-role-mitogen)|
|13|[ansible-role-mssql](https://travis-ci.org/robertdebock/ansible-role-mssql)|
|13|[ansible-role-mysql](https://travis-ci.org/robertdebock/ansible-role-mysql)|
|14|[ansible-role-natrouter](https://travis-ci.org/robertdebock/ansible-role-natrouter)|
|14|[ansible-role-nginx](https://travis-ci.org/robertdebock/ansible-role-nginx)|
|14|[ansible-role-npm](https://travis-ci.org/robertdebock/ansible-role-npm)|
|14|[ansible-role-ntp](https://travis-ci.org/robertdebock/ansible-role-ntp)|
|15|[ansible-role-owncloud](https://travis-ci.org/robertdebock/ansible-role-owncloud)|
|16|[ansible-role-packer](https://travis-ci.org/robertdebock/ansible-role-packer)|
|16|[ansible-role-php](https://travis-ci.org/robertdebock/ansible-role-php)|
|16|[ansible-role-phpmyadmin](https://travis-ci.org/robertdebock/ansible-role-phpmyadmin)|
|16|[ansible-role-postfix](https://travis-ci.org/robertdebock/ansible-role-postfix)|
|16|[ansible-role-python-pip](https://travis-ci.org/robertdebock/ansible-role-python-pip)|
|18|[ansible-role-release](https://travis-ci.org/robertdebock/ansible-role-release)|
|18|[ansible-role-revealmd](https://travis-ci.org/robertdebock/ansible-role-revealmd)|
|18|[ansible-role-roundcubemail](https://travis-ci.org/robertdebock/ansible-role-roundcubemail)|
|18|[ansible-role-rsyslog](https://travis-ci.org/robertdebock/ansible-role-rsyslog)|
|18|[ansible-role-ruby](https://travis-ci.org/robertdebock/ansible-role-ruby)|
|18|[ansible-role-rundeck](https://travis-ci.org/robertdebock/ansible-role-rundeck)|
|19|[ansible-role-scl](https://travis-ci.org/robertdebock/ansible-role-scl)|
|19|[ansible-role-spamassassin](https://travis-ci.org/robertdebock/ansible-role-spamassassin)|
|19|[ansible-role-sudo-pair](https://travis-ci.org/robertdebock/ansible-role-sudo-pair)|
|20|[ansible-role-terraform](https://travis-ci.org/robertdebock/ansible-role-terraform)|
|20|[ansible-role-tftpd](https://travis-ci.org/robertdebock/ansible-role-tftpd)|
|20|[ansible-role-tomcat](https://travis-ci.org/robertdebock/ansible-role-tomcat)|
|21|[ansible-role-update](https://travis-ci.org/robertdebock/ansible-role-update)|
|24|[ansible-role-xinetd](https://travis-ci.org/robertdebock/ansible-role-xinetd)|
|26|[ansible-role-zabbix](https://travis-ci.org/robertdebock/ansible-role-zabbix)|

## Integration

To test a combination of Ansible roles, Travis CI runs terraform and a complex playbook.

There is currently one [integration test](https://travis-ci.org/robertdebock/ansible-integration). A [report](https://robertdebock.nl/ansible-integration/) is saved every run.
