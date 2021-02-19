# [Tests](#tests)

The filosofy to test is:
- Test multiple distributions. (see .travis.yml)
- Test multiple version of Ansible, previous, current and next previous and future. (see tox.ini)

In Travis CI these combinations are called a `matrix`. You can consider this overview per role:

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

Read [this page to understand the tools (Travis, Molecule and Tox)](tox-molecule-travis.html] better.

There are multiple tests configured, here is how they relate.

## [Unit tests](#unit-tests)

To test an Ansible role, Travis CI runs molecule on a commit. This verifies that the role does it's job, but does not ensure that it works in combination with other roles.

An [example for the unit test for the Ansible role java](https://travis-ci.com/robertdebock/ansible-role-java).

### [Time based unit tests](#time-based-unit-tests)

Because distributions, molecule, and ansible change over time, a monthly test is done to all roles using this schedule:

|------------|------------|
|Day of month|Ansible Role|
|1|[aide](https://travis-ci.com/robertdebock/ansible-role-aide/settings)|
|1|[alternatives](https://travis-ci.com/robertdebock/ansible-role-alternatives/settings)|
|1|[anaconda](https://travis-ci.com/robertdebock/ansible-role-anaconda/settings)|
|1|[ansible](https://travis-ci.com/robertdebock/ansible-role-ansible/settings)|
|1|[ansible_lint](https://travis-ci.com/robertdebock/ansible-role-ansible_lint/settings)|
|1|[apt_autostart](https://travis-ci.com/robertdebock/ansible-role-apt_autostart/settings)|
|1|[apt_repository](https://travis-ci.com/robertdebock/ansible-role-apt_repository/settings)|
|1|[ara](https://travis-ci.com/robertdebock/ansible-role-ara/settings)|
|1|[artifactory](https://travis-ci.com/robertdebock/ansible-role-artifactory/settings)|
|1|[at](https://travis-ci.com/robertdebock/ansible-role-at/settings)|
|1|[atom](https://travis-ci.com/robertdebock/ansible-role-atom/settings)|
|1|[auditd](https://travis-ci.com/robertdebock/ansible-role-auditd/settings)|
|1|[autofs](https://travis-ci.com/robertdebock/ansible-role-autofs/settings)|
|1|[auto_update](https://travis-ci.com/robertdebock/ansible-role-auto_update/settings)|
|1|[azure_cli](https://travis-ci.com/robertdebock/ansible-role-azure_cli/settings)|
|2|[backup](https://travis-ci.com/robertdebock/ansible-role-backup/settings)|
|2|[bios_update](https://travis-ci.com/robertdebock/ansible-role-bios_update/settings)|
|2|[bootstrap](https://travis-ci.com/robertdebock/ansible-role-bootstrap/settings)|
|2|[buildtools](https://travis-ci.com/robertdebock/ansible-role-buildtools/settings)|
|3|[ca](https://travis-ci.com/robertdebock/ansible-role-ca/settings)|
|3|[ca_certificates](https://travis-ci.com/robertdebock/ansible-role-ca_certificates/settings)|
|3|[cargo](https://travis-ci.com/robertdebock/ansible-role-cargo/settings)|
|3|[certbot](https://travis-ci.com/robertdebock/ansible-role-certbot/settings)|
|3|[cis](https://travis-ci.com/robertdebock/ansible-role-cis/settings)|
|3|[cntlm](https://travis-ci.com/robertdebock/ansible-role-cntlm/settings)|
|3|[collectd](https://travis-ci.com/robertdebock/ansible-role-collectd/settings)|
|3|[common](https://travis-ci.com/robertdebock/ansible-role-common/settings)|
|3|[container_docs](https://travis-ci.com/robertdebock/ansible-role-container_docs/settings)|
|3|[core_dependencies](https://travis-ci.com/robertdebock/ansible-role-core_dependencies/settings)|
|3|[cron](https://travis-ci.com/robertdebock/ansible-role-cron/settings)|
|3|**[cups](https://travis-ci.com/robertdebock/ansible-role-cups/settings)**|
|3|[clamav](https://travis-ci.com/robertdebock/ansible-role-clamav/settings)|
|4|[debug](https://travis-ci.com/robertdebock/ansible-role-debug/settings)|
|4|[dhcpd](https://travis-ci.com/robertdebock/ansible-role-dhcpd/settings)|
|4|[digitalocean_agent](https://travis-ci.com/robertdebock/ansible-role-digitalocean-agent/settings)|
|4|[dns](https://travis-ci.com/robertdebock/ansible-role-dns/settings)|
|4|**[dnsmasq](https://travis-ci.com/robertdebock/ansible-role-dnsmasq/settings)**|
|4|[docker](https://travis-ci.com/robertdebock/ansible-role-docker/settings)|
|4|[docker_ce](https://travis-ci.com/robertdebock/ansible-role-docker_ce/settings)|
|4|**[docker_compose](https://travis-ci.com/robertdebock/ansible-role-docker_compose/settings)**|
|4|[dovecot](https://travis-ci.com/robertdebock/ansible-role-dovecot/settings)|
|4|[dsvpn](https://travis-ci.com/robertdebock/ansible-role-dsvpn/settings)|
|5|[earlyoom](https://travis-ci.com/robertdebock/ansible-role-earlyoom/settings)|
|5|[eclipse](https://travis-ci.com/robertdebock/ansible-role-eclipse/settings)|
|5|[environment](https://travis-ci.com/robertdebock/ansible-role-environment/settings)|
|5|[epel](https://travis-ci.com/robertdebock/ansible-role-epel/settings)|
|5|[etherpad](https://travis-ci.com/robertdebock/ansible-role-etherpad/settings)|
|6|[facts](https://travis-ci.com/robertdebock/ansible-role-facts/settings)|
|6|[fail2ban](https://travis-ci.com/robertdebock/ansible-role-fail2ban/settings)|
|6|[filesystem](https://travis-ci.com/robertdebock/ansible-role-filesystem/settings)|
|6|[firewall](https://travis-ci.com/robertdebock/ansible-role-firewall/settings)|
|6|[forensics](https://travis-ci.com/robertdebock/ansible-role-forensics/settings)|
|6|[functions](https://travis-ci.com/robertdebock/ansible-role-functions/settings)|
|7|[git](https://travis-ci.com/robertdebock/ansible-role-git/settings)|
|7|[gitlab_runner](https://travis-ci.com/robertdebock/ansible-role-gitlab_runner/settings)|
|7|[glusterfs](https://travis-ci.com/robertdebock/ansible-role-glusterfs/settings)|
|7|[gnome](https://travis-ci.com/robertdebock/ansible-role-gnome/settings)|
|7|[grub](https://travis-ci.com/robertdebock/ansible-role-grub/settings)|
|7|[go](https://travis-ci.com/robertdebock/ansible-role-go/settings)|
|7|[gotop](https://travis-ci.com/robertdebock/ansible-role-gotop/settings)|
|8|[haproxy](https://travis-ci.com/robertdebock/ansible-role-haproxy/settings)|
|8|**[hashicorp](https://travis-ci.com/robertdebock/ansible-role-hashicorp/settings)**|
|8|[haveged](https://travis-ci.com/robertdebock/ansible-role-haveged/settings)|
|8|[httpd](https://travis-ci.com/robertdebock/ansible-role-httpd/settings)|
|8|[hostname](https://travis-ci.com/robertdebock/ansible-role-hostname/settings)|
|9|[investigate](https://travis-ci.com/robertdebock/ansible-role-investigate/settings)|
|9|[irslackd](https://travis-ci.com/robertdebock/ansible-role-irslackd/settings)|
|10|[java](https://travis-ci.com/robertdebock/ansible-role-java/settings)|
|10|[jenkins](https://travis-ci.com/robertdebock/ansible-role-jenkins/settings)|
|10|[jitsi](https://travis-ci.com/robertdebock/ansible-role-jitsi/settings)|
|11|[keepalived](https://travis-ci.com/robertdebock/ansible-role-keepalived/settings)|
|11|[kernel](https://travis-ci.com/robertdebock/ansible-role-kernel/settings)|
|11|[kubectl](https://travis-ci.com/robertdebock/ansible-role-kubectl/settings)|
|12|[libvirt](https://travis-ci.com/robertdebock/ansible-role-libvirt/settings)|
|12|[logrotate](https://travis-ci.com/robertdebock/ansible-role-logrotate/settings)|
|12|[logwatch](https://travis-ci.com/robertdebock/ansible-role-logwatch/settings)|
|12|[luks](https://travis-ci.com/robertdebock/ansible-role-luks/settings)|
|12|[lvm](https://travis-ci.com/robertdebock/ansible-role-lvm/settings)|
|12|[locale](https://travis-ci.com/robertdebock/ansible-role-locale/settings)|
|12|[lynis](https://travis-ci.com/robertdebock/ansible-role-lynis/settings)|
|13|[memcached](https://travis-ci.com/robertdebock/ansible-role-memcached/settings)|
|13|[maintenance](https://travis-ci.com/robertdebock/ansible-role-maintenance/settings)|
|13|[mate](https://travis-ci.com/robertdebock/ansible-role-mate/settings)|
|13|[mediawiki](https://travis-ci.com/robertdebock/ansible-role-mediawiki/settings)|
|13|[microsoft_repository_keys](https://travis-ci.com/robertdebock/ansible-role-microsoft_repository_keys/settings)|
|13|[mitogen](https://travis-ci.com/robertdebock/ansible-role-mitogen/settings)|
|13|[modprobe](https://travis-ci.com/robertdebock/ansible-role-modprobe/settings)|
|13|[minikube](https://travis-ci.com/robertdebock/ansible-role-minikube/settings)|
|13|[molecule](https://travis-ci.com/robertdebock/ansible-role-molecule/settings)|
|13|**[moodle](https://travis-ci.com/robertdebock/ansible-role-moodle/settings)**|
|13|[mount](https://travis-ci.com/robertdebock/ansible-role-mount/settings)|
|13|[mssql](https://travis-ci.com/robertdebock/ansible-role-mssql/settings)|
|13|[mysql](https://travis-ci.com/robertdebock/ansible-role-mysql/settings)|
|14|[natrouter](https://travis-ci.com/robertdebock/ansible-role-natrouter/settings)|
|14|[nextcloud](https://travis-ci.com/robertdebock/ansible-role-nextcloud/settings)|
|14|[nfsserver](https://travis-ci.com/robertdebock/ansible-role-nfsserver/settings)|
|14|[nginx](https://travis-ci.com/robertdebock/ansible-role-nginx/settings)|
|14|[node_red](https://travis-ci.com/robertdebock/ansible-role-node_red/settings)|
|14|[nomad](https://travis-ci.com/robertdebock/ansible-role-nomad/settings)|
|14|[npm](https://travis-ci.com/robertdebock/ansible-role-npm/settings)|
|14|[ntp](https://travis-ci.com/robertdebock/ansible-role-ntp/settings)|
|15|[obsproject](https://travis-ci.com/robertdebock/ansible-role-obsproject/settings)|
|15|[omsagent](https://travis-ci.com/robertdebock/ansible-role-omsagent/settings)|
|15|[openssh](https://travis-ci.com/robertdebock/ansible-role-openssh/settings)|
|15|[openssl](https://travis-ci.com/robertdebock/ansible-role-openssl/settings)|
|15|[owncloud](https://travis-ci.com/robertdebock/ansible-role-owncloud/settings)|
|16|[packer](https://travis-ci.com/robertdebock/ansible-role-packer/settings)|
|16|[php](https://travis-ci.com/robertdebock/ansible-role-php/settings)|
|16|[php_fpm](https://travis-ci.com/robertdebock/ansible-role-php_fpm/settings)|
|16|[phpmyadmin](https://travis-ci.com/robertdebock/ansible-role-phpmyadmin/settings)|
|16|[postfix](https://travis-ci.com/robertdebock/ansible-role-postfix/settings)|
|16|[postgres](https://travis-ci.com/robertdebock/ansible-role-postgres/settings)|
|16|[powertop](https://travis-ci.com/robertdebock/ansible-role-powertop/settings)|
|16|[powertools](https://travis-ci.com/robertdebock/ansible-role-powertools/settings)|
|16|[python_pip](https://travis-ci.com/robertdebock/ansible-role-python_pip/settings)|
|18|[qemu](https://travis-ci.com/robertdebock/ansible-role-qemu/settings)|
|18|[reboot](https://travis-ci.com/robertdebock/ansible-role-reboot/settings)|
|18|[redis](https://travis-ci.com/robertdebock/ansible-role-redis/settings)|
|18|[release](https://travis-ci.com/robertdebock/ansible-role-release/settings)|
|18|[remi](https://travis-ci.com/robertdebock/ansible-role-remi/settings)|
|18|[restore](https://travis-ci.com/robertdebock/ansible-role-restore/settings)|
|18|[revealmd](https://travis-ci.com/robertdebock/ansible-role-revealmd/settings)|
|18|[roundcubemail](https://travis-ci.com/robertdebock/ansible-role-roundcubemail/settings)|
|18|[rpmfusion](https://travis-ci.com/robertdebock/ansible-role-rpmfusion/settings)|
|18|[rsyslog](https://travis-ci.com/robertdebock/ansible-role-rsyslog/settings)|
|18|[ruby](https://travis-ci.com/robertdebock/ansible-role-ruby/settings)|
|18|[rundeck](https://travis-ci.com/robertdebock/ansible-role-rundeck/settings)|
|19|[scl](https://travis-ci.com/robertdebock/ansible-role-scl/settings)|
|19|[selinux](https://travis-ci.com/robertdebock/ansible-role-selinux/settings)|
|19|[service](https://travis-ci.com/robertdebock/ansible-role-service/settings)|
|19|[snort](https://travis-ci.com/robertdebock/ansible-role-snort/settings)|
|19|[software](https://travis-ci.com/robertdebock/ansible-role-software/settings)|
|19|[sosreport](https://travis-ci.com/robertdebock/ansible-role-sosreport/settings)|
|19|[spamassassin](https://travis-ci.com/robertdebock/ansible-role-spamassassin/settings)|
|19|[squid](https://travis-ci.com/robertdebock/ansible-role-squid/settings)|
|19|[storage](https://travis-ci.com/robertdebock/ansible-role-storage/settings)|
|19|[stratis](https://travis-ci.com/robertdebock/ansible-role-stratis/settings)|
|19|[subversion](https://travis-ci.com/robertdebock/ansible-role-subversion/settings)|
|19|[sudo-pair](https://travis-ci.com/robertdebock/ansible-role-sudo-pair/settings)|
|19|[swap](https://travis-ci.com/robertdebock/ansible-role-swap/settings)|
|19|[sysctl](https://travis-ci.com/robertdebock/ansible-role-sysctl/settings)|
|19|[sysstat](https://travis-ci.com/robertdebock/ansible-role-sysstat/settings)|
|19|[systemd](https://travis-ci.com/robertdebock/ansible-role-systemd/settings)|
|20|[terraform](https://travis-ci.com/robertdebock/ansible-role-terraform/settings)|
|20|[test_connection](https://travis-ci.com/robertdebock/ansible-role-test_connection/settings)|
|20|[tftpd](https://travis-ci.com/robertdebock/ansible-role-tftpd/settings)|
|20|[tigervnc](https://travis-ci.com/robertdebock/ansible-role-tigervnc/settings)|
|20|[tlp](https://travis-ci.com/robertdebock/ansible-role-tlp/settings)|
|20|[tomcat](https://travis-ci.com/robertdebock/ansible-role-tomcat/settings)|
|20|[travis](https://travis-ci.com/robertdebock/ansible-role-travis/settings)|
|20|[tune2fs](https://travis-ci.com/robertdebock/ansible-role-tune2fs/settings)|
|21|[types](https://travis-ci.com/robertdebock/ansible-role-types/settings)|
|21|[ulimit](https://travis-ci.com/robertdebock/ansible-role-ulimit/settings)|
|21|[umask](https://travis-ci.com/robertdebock/ansible-role-umask/settings)|
|21|[update](https://travis-ci.com/robertdebock/ansible-role-update/settings)|
|21|[update_package_cache](https://travis-ci.com/robertdebock/ansible-role-update_package_cache/settings)|
|21|[upgrade](https://travis-ci.com/robertdebock/ansible-role-upgrade/settings)|
|21|[unbound](https://travis-ci.com/robertdebock/ansible-role-unbound/settings)|
|21|[unowned_files](https://travis-ci.com/robertdebock/ansible-role-unowned_files/settings)|
|21|[users](https://travis-ci.com/robertdebock/ansible-role-users/settings)|
|22|[vagrant](https://travis-ci.com/robertdebock/ansible-role-vagrant/settings)|
|22|[vault](https://travis-ci.com/robertdebock/ansible-role-vault/settings)|
|22|[vdo](https://travis-ci.com/robertdebock/ansible-role-vdo/settings)|
|22|[virtualbox](https://travis-ci.com/robertdebock/ansible-role-virtualbox/settings)|
|24|[xinetd](https://travis-ci.com/robertdebock/ansible-role-xinetd/settings)|
|24|[xrdp](https://travis-ci.com/robertdebock/ansible-role-xrdp/settings)|
|25|[y](https://travis-ci.com/robertdebock/ansible-role-y/settings)|
|26|[zabbix_agent](https://travis-ci.com/robertdebock/ansible-role-zabbix_agent/settings)|
|26|[zabbix_proxy](https://travis-ci.com/robertdebock/ansible-role-zabbix_proxy/settings)|
|26|[zabbix_repository](https://travis-ci.com/robertdebock/ansible-role-zabbix_repository/settings)|
|26|[zabbix_server](https://travis-ci.com/robertdebock/ansible-role-zabbix_server/settings)|
|26|[zabbix_web](https://travis-ci.com/robertdebock/ansible-role-zabbix_web/settings)|

## [Integration](#integration)

To test a combination of Ansible roles, Travis CI runs terraform and a complex playbook.

There is currently one [integration test](https://travis-ci.com/robertdebock/ansible-integration). A [report](https://robertdebock.nl/ansible-integration/) is saved every run.
