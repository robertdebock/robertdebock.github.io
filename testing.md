# Tests

The filosofy to test is:
- Test multiple distributions. (see .travis.yml)
- Test multiple version of Ansible, previous, current and next previous and future. (see tox.ini)

In Travis CI these combinations are called a `matrix`. You can consider this overview per role:

| Distribution        | Ansible 2.8 | Ansible 2.9 | Ansible devel |
|---------------------|-------------|-------------|---------------|
| Alpine latest       | yes         | yes         | yes           |
| Alpine edge         | yes *       | yes *       | yes           |
| Archlinux (base)    | yes         | yes         | yes           |
| CentOS 7            | yes         | yes         | yes           |
| CentOS latest       | yes         | yes         | yes           |
| Debian stable       | yes         | yes         | yes           |
| Debian latest       | yes         | yes         | yes           |
| Debian unstable     | yes *       | yes *       | yes           |
| Fedora latest       | yes         | yes         | yes           |
| Fedora rawhide      | yes *       | yes         | yes           |
| OpenSuse Leap       | yes         | yes         | yes           |
| OpenSuse Tumbleweed | yes         | yes         | yes           |
| RHEL 7              | yes         | yes         | yes           |
| RHEL 8              | yes         | yes         | yes           |
| Ubuntu Artful (17)  | yes         | yes         | yes           |
| Ubuntu latest       | yes         | yes         | yes           |
| Ubuntu devel        | yes *       | yes *       | yes           |

Distributions or Ansible versions marked with an astriks are allowed to fail. This combination is built to prepare for future distributions or releases of Ansible.

Read [this page to understand the tools (Travis, Molecule and Tox)](tox-molecule-travis.html] better.

There are multiple tests configured, here is how they relate.

## Unit tests

To test an Ansible role, Travis CI runs molecule on a commit. This verifies that the role does it's job, but does not ensure that it works in combination with other roles.

An [example for the unit test for the Ansible role java](https://travis-ci.org/robertdebock/ansible-role-java).

### Time based unit tests

Because distriutions, molecule, ansible and goss change over time, a monthly test is done to all roles using this schedule:

|------------|------------|
|Day of month|Ansible Role|
|1|[aide](https://travis-ci.org/robertdebock/ansible-role-aide/settings)|
|1|[alternatives](https://travis-ci.org/robertdebock/ansible-role-alternatives/settings)|
|1|[anaconda](https://travis-ci.org/robertdebock/ansible-role-anaconda/settings)|
|1|[ansible](https://travis-ci.org/robertdebock/ansible-role-ansible/settings)|
|1|[ansible_lint](https://travis-ci.org/robertdebock/ansible-role-ansible_lint/settings)|
|1|[apt_autostart](https://travis-ci.org/robertdebock/ansible-role-apt_autostart/settings)|
|1|[ara](https://travis-ci.org/robertdebock/ansible-role-ara/settings)|
|1|[artifactory](https://travis-ci.org/robertdebock/ansible-role-artifactory/settings)|
|1|[at](https://travis-ci.org/robertdebock/ansible-role-at/settings)|
|1|[atom](https://travis-ci.org/robertdebock/ansible-role-atom/settings)|
|1|[auditd](https://travis-ci.org/robertdebock/ansible-role-auditd/settings)|
|1|[auto_update](https://travis-ci.org/robertdebock/ansible-role-auto_update/settings)|
|1|[awx](https://travis-ci.org/robertdebock/ansible-role-awx/settings)|
|1|[azure_cli](https://travis-ci.org/robertdebock/ansible-role-azure_cli/settings)|
|2|[backup](https://travis-ci.org/robertdebock/ansible-role-backup/settings)|
|2|[bios_update](https://travis-ci.org/robertdebock/ansible-role-bios_update/settings)|
|2|[bootstrap](https://travis-ci.org/robertdebock/ansible-role-bootstrap/settings)|
|2|[buildtools](https://travis-ci.org/robertdebock/ansible-role-buildtools/settings)|
|3|[ca](https://travis-ci.org/robertdebock/ansible-role-ca/settings)|
|3|[ca_certificates](https://travis-ci.org/robertdebock/ansible-role-ca_certificates/settings)|
|3|[cargo](https://travis-ci.org/robertdebock/ansible-role-cargo/settings)|
|3|[cntlm](https://travis-ci.org/robertdebock/ansible-role-cntlm/settings)|
|3|[collectd](https://travis-ci.org/robertdebock/ansible-role-collectd/settings)|
|3|[common](https://travis-ci.org/robertdebock/ansible-role-common/settings)|
|3|[container_docs](https://travis-ci.org/robertdebock/ansible-role-container_docs/settings)|
|3|[core_dependencies](https://travis-ci.org/robertdebock/ansible-role-core_dependencies/settings)|
|3|[cron](https://travis-ci.org/robertdebock/ansible-role-cron/settings)|
|3|[clamav](https://travis-ci.org/robertdebock/ansible-role-clamav/settings)|
|4|**[debug](https://travis-ci.org/robertdebock/ansible-role-debug/settings)**|
|4|[dhcpd](https://travis-ci.org/robertdebock/ansible-role-dhcpd/settings)|
|4|[digitalocean_agent](https://travis-ci.org/robertdebock/ansible-role-digitalocean-agent/settings)|
|4|[dns](https://travis-ci.org/robertdebock/ansible-role-dns/settings)|
|4|[docker](https://travis-ci.org/robertdebock/ansible-role-docker/settings)|
|4|[docker_ce](https://travis-ci.org/robertdebock/ansible-role-docker_ce/settings)|
|4|[dovecot](https://travis-ci.org/robertdebock/ansible-role-dovecot/settings)|
|4|[dsvpn](https://travis-ci.org/robertdebock/ansible-role-dsvpn/settings)|
|5|[earlyoom](https://travis-ci.org/robertdebock/ansible-role-earlyoom/settings)|
|5|**[eclipse](https://travis-ci.org/robertdebock/ansible-role-eclipse/settings)**|
|5|[environment](https://travis-ci.org/robertdebock/ansible-role-environment/settings)|
|5|[epel](https://travis-ci.org/robertdebock/ansible-role-epel/settings)|
|5|[etherpad](https://travis-ci.org/robertdebock/ansible-role-etherpad/settings)|
|6|**[facts](https://travis-ci.org/robertdebock/ansible-role-facts/settings)**|
|6|[fail2ban](https://travis-ci.org/robertdebock/ansible-role-fail2ban/settings)|
|6|[firewall](https://travis-ci.org/robertdebock/ansible-role-firewall/settings)|
|6|**[forensics](https://travis-ci.org/robertdebock/ansible-role-forensics/settings)**|
|7|[git](https://travis-ci.org/robertdebock/ansible-role-git/settings)|
|7|[gitlab_runner](https://travis-ci.org/robertdebock/ansible-role-gitlab_runner/settings)|
|7|[glusterfs](https://travis-ci.org/robertdebock/ansible-role-glusterfs/settings)|
|7|[go](https://travis-ci.org/robertdebock/ansible-role-go/settings)|
|7|[gotop](https://travis-ci.org/robertdebock/ansible-role-gotop/settings)|
|8|[haproxy](https://travis-ci.org/robertdebock/ansible-role-haproxy/settings)|
|8|[haveged](https://travis-ci.org/robertdebock/ansible-role-haveged/settings)|
|8|[httpd](https://travis-ci.org/robertdebock/ansible-role-httpd/settings)|
|8|[hostname](https://travis-ci.org/robertdebock/ansible-role-hostname/settings)|
|9|[investigate](https://travis-ci.org/robertdebock/ansible-role-investigate/settings)|
|9|[irslackd](https://travis-ci.org/robertdebock/ansible-role-irslackd/settings)|
|10|[java](https://travis-ci.org/robertdebock/ansible-role-java/settings)|
|10|[jenkins](https://travis-ci.org/robertdebock/ansible-role-jenkins/settings)|
|11|[kernel](https://travis-ci.org/robertdebock/ansible-role-kernel/settings)|
|11|[kubectl](https://travis-ci.org/robertdebock/ansible-role-kubectl/settings)|
|12|[logrotate](https://travis-ci.org/robertdebock/ansible-role-logrotate/settings)|
|12|[logwatch](https://travis-ci.org/robertdebock/ansible-role-logwatch/settings)|
|12|[locale](https://travis-ci.org/robertdebock/ansible-role-locale/settings)|
|12|[lynis](https://travis-ci.org/robertdebock/ansible-role-lynis/settings)|
|13|[memcached](https://travis-ci.org/robertdebock/ansible-role-memcached/settings)|
|13|[mediawiki](https://travis-ci.org/robertdebock/ansible-role-mediawiki/settings)|
|13|[microsoft_repository_keys](https://travis-ci.org/robertdebock/ansible-role-microsoft_repository_keys/settings)|
|13|[mitogen](https://travis-ci.org/robertdebock/ansible-role-mitogen/settings)|
|13|[minikube](https://travis-ci.org/robertdebock/ansible-role-minikube/settings)|
|13|[molecule](https://travis-ci.org/robertdebock/ansible-role-molecule/settings)|
|13|[mssql](https://travis-ci.org/robertdebock/ansible-role-mssql/settings)|
|13|[mysql](https://travis-ci.org/robertdebock/ansible-role-mysql/settings)|
|14|[natrouter](https://travis-ci.org/robertdebock/ansible-role-natrouter/settings)|
|14|[nginx](https://travis-ci.org/robertdebock/ansible-role-nginx/settings)|
|14|[npm](https://travis-ci.org/robertdebock/ansible-role-npm/settings)|
|14|[ntp](https://travis-ci.org/robertdebock/ansible-role-ntp/settings)|
|15|[obsproject](https://travis-ci.org/robertdebock/ansible-role-obsproject/settings)|
|15|[omsagent](https://travis-ci.org/robertdebock/ansible-role-omsagent/settings)|
|15|[openssh](https://travis-ci.org/robertdebock/ansible-role-openssh/settings)|
|15|[owncloud](https://travis-ci.org/robertdebock/ansible-role-owncloud/settings)|
|16|[packer](https://travis-ci.org/robertdebock/ansible-role-packer/settings)|
|16|[php](https://travis-ci.org/robertdebock/ansible-role-php/settings)|
|16|[php_fpm](https://travis-ci.org/robertdebock/ansible-role-php_fpm/settings)|
|16|[phpmyadmin](https://travis-ci.org/robertdebock/ansible-role-phpmyadmin/settings)|
|16|[postfix](https://travis-ci.org/robertdebock/ansible-role-postfix/settings)|
|16|[postgres](https://travis-ci.org/robertdebock/ansible-role-postgres/settings)|
|16|[powertop](https://travis-ci.org/robertdebock/ansible-role-powertop/settings)|
|16|**[powertools](https://travis-ci.org/robertdebock/ansible-role-powertools/settings)**|
|16|[python_pip](https://travis-ci.org/robertdebock/ansible-role-python-pip/settings)|
|18|[reboot](https://travis-ci.org/robertdebock/ansible-role-reboot/settings)|
|18|[redis](https://travis-ci.org/robertdebock/ansible-role-redis/settings)|
|18|[release](https://travis-ci.org/robertdebock/ansible-role-release/settings)|
|18|[remi](https://travis-ci.org/robertdebock/ansible-role-remi/settings)|
|18|[restore](https://travis-ci.org/robertdebock/ansible-role-restore/settings)|
|18|[revealmd](https://travis-ci.org/robertdebock/ansible-role-revealmd/settings)|
|18|[roundcubemail](https://travis-ci.org/robertdebock/ansible-role-roundcubemail/settings)|
|18|[rpmfusion](https://travis-ci.org/robertdebock/ansible-role-rpmfusion/settings)|
|18|[rsyslog](https://travis-ci.org/robertdebock/ansible-role-rsyslog/settings)|
|18|[ruby](https://travis-ci.org/robertdebock/ansible-role-ruby/settings)|
|18|[rundeck](https://travis-ci.org/robertdebock/ansible-role-rundeck/settings)|
|19|[scl](https://travis-ci.org/robertdebock/ansible-role-scl/settings)|
|19|[selinux](https://travis-ci.org/robertdebock/ansible-role-selinux/settings)|
|19|[service](https://travis-ci.org/robertdebock/ansible-role-service/settings)|
|19|[snort](https://travis-ci.org/robertdebock/ansible-role-snort/settings)|
|19|[sosreport](https://travis-ci.org/robertdebock/ansible-role-sosreport/settings)|
|19|[spamassassin](https://travis-ci.org/robertdebock/ansible-role-spamassassin/settings)|
|19|[squid](https://travis-ci.org/robertdebock/ansible-role-squid/settings)|
|19|[storage](https://travis-ci.org/robertdebock/ansible-role-storage/settings)|
|19|[stratis](https://travis-ci.org/robertdebock/ansible-role-stratis/settings)|
|19|[subversion](https://travis-ci.org/robertdebock/ansible-role-subversion/settings)|
|19|[sudo-pair](https://travis-ci.org/robertdebock/ansible-role-sudo-pair/settings)|
|19|[sysctl](https://travis-ci.org/robertdebock/ansible-role-sysctl/settings)|
|19|[sysstat](https://travis-ci.org/robertdebock/ansible-role-sysstat/settings)|
|20|[terraform](https://travis-ci.org/robertdebock/ansible-role-terraform/settings)|
|20|[test_connection](https://travis-ci.org/robertdebock/ansible-role-test_connection/settings)|
|20|[tftpd](https://travis-ci.org/robertdebock/ansible-role-tftpd/settings)|
|20|[tomcat](https://travis-ci.org/robertdebock/ansible-role-tomcat/settings)|
|20|[travis](https://travis-ci.org/robertdebock/ansible-role-travis/settings)|
|21|[update](https://travis-ci.org/robertdebock/ansible-role-update/settings)|
|21|[update_package_cache](https://travis-ci.org/robertdebock/ansible-role-update_package_cache/settings)|
|21|[ulimit](https://travis-ci.org/robertdebock/ansible-role-ulimit/settings)|
|21|[unbound](https://travis-ci.org/robertdebock/ansible-role-unbound/settings)|
|21|[unowned_files](https://travis-ci.org/robertdebock/ansible-role-unowned_files/settings)|
|21|[users](https://travis-ci.org/robertdebock/ansible-role-users/settings)|
|22|[vagrant](https://travis-ci.org/robertdebock/ansible-role-vagrant/settings)|
|24|[xinetd](https://travis-ci.org/robertdebock/ansible-role-xinetd/settings)|
|25|[y](https://travis-ci.org/robertdebock/ansible-role-y/settings)|
|26|[zabbix_agent](https://travis-ci.org/robertdebock/ansible-role-zabbix_agent/settings)|
|26|[zabbix_proxy](https://travis-ci.org/robertdebock/ansible-role-zabbix_proxy/settings)|
|26|[zabbix_repository](https://travis-ci.org/robertdebock/ansible-role-zabbix_repository/settings)|
|26|[zabbix_server](https://travis-ci.org/robertdebock/ansible-role-zabbix_server/settings)|
|26|[zabbix_web](https://travis-ci.org/robertdebock/ansible-role-zabbix_web/settings)|

## Integration

To test a combination of Ansible roles, Travis CI runs terraform and a complex playbook.

There is currently one [integration test](https://travis-ci.org/robertdebock/ansible-integration). A [report](https://robertdebock.nl/ansible-integration/) is saved every run.
