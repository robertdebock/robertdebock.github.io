# Tests

The filosofy to test is:
- Test multiple distributions
- Of each distribution, test the current and previous version
- Test multiple version of Ansible, current, previous and next previous and future.

In Travis CI these combinations are called a `matrix`. You can consider this overview per role:

| Distribution        | Ansible 2.6 | Ansible 2.7 | Ansible devel |
|---------------------|-------------|-------------|---------------|
| Alpine latest       | yes         | yes         | yes *         |
| Alpine edge         | yes *       | yes *       | yes *         |
| Archlinux (base)    | yes         | yes         | yes *         |
| Centos 6            | yes         | yes         | yes *         |
| Centos latest       | yes         | yes         | yes *         |
| Debian stable       | yes         | yes         | yes *         |
| Debian latest       | yes         | yes         | yes *         |
| Debian unstable     | yes *       | yes *       | yes *         |
| Fedora latest       | yes         | yes         | yes *         |
| Fedora rawhide      | yes *       | yes         | yes *         |
| OpenSuse Leap       | yes         | yes         | yes *         |
| OpenSuse Tumbleweed | yes         | yes         | yes *         |
| Ubuntu Artful (17)  | yes         | yes         | yes *         |
| Ubuntu latest       | yes         | yes         | yes *         |
| Ubuntu devel        | yes *       | yes *       | yes *         |

This matrix runs 15 (distributions) times 3 (Ansible versions) equals 45 build.

Distributions or Ansible versions marked with an astriks are allowed to fail. This combination is built to prepare for future distributions or releases of Ansible.

If a distribution or ansible version is not supported, the strategy is to also test that, and ensure if fails.

There are multiple tests configured, here is how they relate.

## Full tests

A container does not offer all features that a full virtual machine offers, for example:
- systemd does not work in many major distributions container.
- rebooting and containers are not a thing.
- Some files in /etc are mounted from the host to the guest.

That's why these roles offer two other molecule scenarios:
- vagrant - to locally start full virtual machines.
- ec2 - to remotely tests commercial full virtual machines.

An overview of the test strategies and their features.

|-------------|-------------------------|
|Scenario name|Tests                    |
|`default`    |Multiple distributions   |
|`vagrant`    |Full virtual machines    |
|`ec2`        |Commercial distributions*|
|travis       |Multiple Ansible versions|

*) The testing of commercial distributions (RHEL & SLES) is not on by default and has to be uncommented.

## Unit tests

To test an Ansible role, Travis CI runs molecule on a commit. This verifies that the role does it's job, but does not ensure that it works in combination with other roles.

An [example for the unit test for the Ansible role java](https://travis-ci.org/robertdebock/ansible-role-java).

### Time based unit tests

Because distriutions, molecule, ansible and goss change over time, a monthly test is done to all roles using this schedule:

|------------|------------|
|Day of month|Ansible Role|
|1|[anaconda](https://travis-ci.org/robertdebock/ansible-role-anaconda/settings)|
|1|[ansible](https://travis-ci.org/robertdebock/ansible-role-ansible/settings)|
|1|[ansible_lint](https://travis-ci.org/robertdebock/ansible-role-ansible_lint/settings)|
|1|[apt_autostart](https://travis-ci.org/robertdebock/ansible-role-apt_autostart/settings)|
|1|[ara](https://travis-ci.org/robertdebock/ansible-role-ara/settings)|
|1|[artifactory](https://travis-ci.org/robertdebock/ansible-role-artifactory/settings)|
|1|[at](https://travis-ci.org/robertdebock/ansible-role-at/settings)|
|1|[atom](https://travis-ci.org/robertdebock/ansible-role-atom/settings)|
|1|[awx](https://travis-ci.org/robertdebock/ansible-role-awx/settings)|
|2|[backup](https://travis-ci.org/robertdebock/ansible-role-backup/settings)|
|2|[bootstrap](https://travis-ci.org/robertdebock/ansible-role-bootstrap/settings)|
|2|[buildtools](https://travis-ci.org/robertdebock/ansible-role-buildtools/settings)|
|3|[ca](https://travis-ci.org/robertdebock/ansible-role-ca/settings)|
|3|[cargo](https://travis-ci.org/robertdebock/ansible-role-cargo/settings)|
|3|[cntlm](https://travis-ci.org/robertdebock/ansible-role-cntlm/settings)|
|3|[common](https://travis-ci.org/robertdebock/ansible-role-common/settings)|
|3|[clamav](https://travis-ci.org/robertdebock/ansible-role-clamav/settings)|
|3|[cloud9](https://travis-ci.org/robertdebock/ansible-role-cloud9/settings)|
|4|[dhcpd](https://travis-ci.org/robertdebock/ansible-role-dhcpd/settings)|
|4|[digitalocean_agent](https://travis-ci.org/robertdebock/ansible-role-digitalocean-agent/settings)|
|4|[dns](https://travis-ci.org/robertdebock/ansible-role-dns/settings)|
|4|[docker](https://travis-ci.org/robertdebock/ansible-role-docker/settings)|
|4|[docker_ce](https://travis-ci.org/robertdebock/ansible-role-docker_ce/settings)|
|4|[dovecot](https://travis-ci.org/robertdebock/ansible-role-dovecot/settings)|
|5|[epel](https://travis-ci.org/robertdebock/ansible-role-epel/settings)|
|5|[etherpad](https://travis-ci.org/robertdebock/ansible-role-etherpad/settings)|
|6|[fail2ban](https://travis-ci.org/robertdebock/ansible-role-fail2ban/settings)|
|6|[firewall](https://travis-ci.org/robertdebock/ansible-role-firewall/settings)|
|7|[git](https://travis-ci.org/robertdebock/ansible-role-git/settings)|
|7|[glusterfs](https://travis-ci.org/robertdebock/ansible-role-glusterfs/settings)|
|7|[go](https://travis-ci.org/robertdebock/ansible-role-go/settings)|
|7|[gotop](https://travis-ci.org/robertdebock/ansible-role-gotop/settings)|
|8|[haproxy](https://travis-ci.org/robertdebock/ansible-role-haproxy/settings)|
|8|[haveged](https://travis-ci.org/robertdebock/ansible-role-haveged/settings)|
|8|[httpd](https://travis-ci.org/robertdebock/ansible-role-httpd/settings)|
|9|[investigate](https://travis-ci.org/robertdebock/ansible-role-investigate/settings)|
|9|[irslackd](https://travis-ci.org/robertdebock/ansible-role-irslackd/settings)|
|10|[java](https://travis-ci.org/robertdebock/ansible-role-java/settings)|
|10|[jenkins](https://travis-ci.org/robertdebock/ansible-role-jenkins/settings)|
|12|[locale](https://travis-ci.org/robertdebock/ansible-role-locale/settings)|
|12|[lynis](https://travis-ci.org/robertdebock/ansible-role-lynis/settings)|
|13|[memcached](https://travis-ci.org/robertdebock/ansible-role-memcached/settings)|
|13|[mediawiki](https://travis-ci.org/robertdebock/ansible-role-mediawiki/settings)|
|13|[mitogen](https://travis-ci.org/robertdebock/ansible-role-mitogen/settings)|
|13|[molecule](https://travis-ci.org/robertdebock/ansible-role-molecule/settings)|
|13|[mssql](https://travis-ci.org/robertdebock/ansible-role-mssql/settings)|
|13|[mysql](https://travis-ci.org/robertdebock/ansible-role-mysql/settings)|
|14|[natrouter](https://travis-ci.org/robertdebock/ansible-role-natrouter/settings)|
|14|[nginx](https://travis-ci.org/robertdebock/ansible-role-nginx/settings)|
|14|[npm](https://travis-ci.org/robertdebock/ansible-role-npm/settings)|
|14|[ntp](https://travis-ci.org/robertdebock/ansible-role-ntp/settings)|
|15|[openssh](https://travis-ci.org/robertdebock/ansible-role-openssh/settings)|
|15|[openvas](https://travis-ci.org/robertdebock/ansible-role-openvas/settings)|
|15|[owncloud](https://travis-ci.org/robertdebock/ansible-role-owncloud/settings)|
|16|[packer](https://travis-ci.org/robertdebock/ansible-role-packer/settings)|
|16|[php](https://travis-ci.org/robertdebock/ansible-role-php/settings)|
|16|[php_fpm](https://travis-ci.org/robertdebock/ansible-role-php_fpm/settings)|
|16|[phpmyadmin](https://travis-ci.org/robertdebock/ansible-role-phpmyadmin/settings)|
|16|[postfix](https://travis-ci.org/robertdebock/ansible-role-postfix/settings)|
|16|[python_pip](https://travis-ci.org/robertdebock/ansible-role-python-pip/settings)|
|18|[reboot](https://travis-ci.org/robertdebock/ansible-role-reboot/settings)|
|18|[redis](https://travis-ci.org/robertdebock/ansible-role-redis/settings)|
|18|[release](https://travis-ci.org/robertdebock/ansible-role-release/settings)|
|18|[restore](https://travis-ci.org/robertdebock/ansible-role-restore/settings)|
|18|[revealmd](https://travis-ci.org/robertdebock/ansible-role-revealmd/settings)|
|18|[roundcubemail](https://travis-ci.org/robertdebock/ansible-role-roundcubemail/settings)|
|18|[rsyslog](https://travis-ci.org/robertdebock/ansible-role-rsyslog/settings)|
|18|[ruby](https://travis-ci.org/robertdebock/ansible-role-ruby/settings)|
|18|[rundeck](https://travis-ci.org/robertdebock/ansible-role-rundeck/settings)|
|19|[scl](https://travis-ci.org/robertdebock/ansible-role-scl/settings)|
|19|[selinux](https://travis-ci.org/robertdebock/ansible-role-selinux/settings)|
|19|**[service](https://travis-ci.org/robertdebock/ansible-role-service/settings)**|
|19|[snort](https://travis-ci.org/robertdebock/ansible-role-snort/settings)|
|19|[spamassassin](https://travis-ci.org/robertdebock/ansible-role-spamassassin/settings)|
|19|[squid](https://travis-ci.org/robertdebock/ansible-role-squid/settings)|
|19|**[storage](https://travis-ci.org/robertdebock/ansible-role-storage/settings)**|
|19|[sudo-pair](https://travis-ci.org/robertdebock/ansible-role-sudo-pair/settings)|
|19|**[sysstat](https://travis-ci.org/robertdebock/ansible-role-sysstat/settings)**|
|20|[terraform](https://travis-ci.org/robertdebock/ansible-role-terraform/settings)|
|20|[tftpd](https://travis-ci.org/robertdebock/ansible-role-tftpd/settings)|
|20|[tomcat](https://travis-ci.org/robertdebock/ansible-role-tomcat/settings)|
|20|[travis](https://travis-ci.org/robertdebock/ansible-role-travis/settings)|
|21|[update](https://travis-ci.org/robertdebock/ansible-role-update/settings)|
|21|[users](https://travis-ci.org/robertdebock/ansible-role-users/settings)|
|22|[vagrant](https://travis-ci.org/robertdebock/ansible-role-vagrant/settings)|
|24|[xinetd](https://travis-ci.org/robertdebock/ansible-role-xinetd/settings)|
|26|[zabbix](https://travis-ci.org/robertdebock/ansible-role-zabbix/settings)|
|26|[zabbix_agent](https://travis-ci.org/robertdebock/ansible-role-zabbix_agent/settings)|
|26|[zabbix_proxy](https://travis-ci.org/robertdebock/ansible-role-zabbix_proxy/settings)|
|26|[zabbix_repository](https://travis-ci.org/robertdebock/ansible-role-zabbix_repository/settings)|
|26|[zabbix_server](https://travis-ci.org/robertdebock/ansible-role-zabbix_server/settings)|
|26|[zabbix_web](https://travis-ci.org/robertdebock/ansible-role-zabbix_web/settings)|

## Integration

To test a combination of Ansible roles, Travis CI runs terraform and a complex playbook.

There is currently one [integration test](https://travis-ci.org/robertdebock/ansible-integration). A [report](https://robertdebock.nl/ansible-integration/) is saved every run.
