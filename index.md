I've written Ansible roles, there are a few things you should know about them.

## Ansible roles
Here is a list of Ansible roles that have been designed to work together.
- [robertdebock.bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap/) ([source](https://github.com/robertdebock/ansible-role-bootstrap)).
- [robertdebock.dhcpd](https://galaxy.ansible.com/robertdebock/dhcpd/) ([source](https://github.com/robertdebock/ansible-role-dhcpd)).
- [robertdebock.dns](https://galaxy.ansible.com/robertdebock/dns/) ([source](https://github.com/robertdebock/ansible-role-dns)).
- [robertdebock.dovecot](https://galaxy.ansible.com/robertdebock/dovecot/) ([source](https://github.com/robertdebock/ansible-role-dovecot)).
- [robertdebock.epel](https://galaxy.ansible.com/robertdebock/epel/) ([source](https://github.com/robertdebock/ansible-role-epel)).
- [robertdebock.fail2ban](https://galaxy.ansible.com/robertdebock/fail2ban/) ([source](https://github.com/robertdebock/ansible-role-fail2ban)).
- [robertdebock.haveged](https://galaxy.ansible.com/robertdebock/haveged/) ([source](https://github.com/robertdebock/ansible-role-haveged)).
- [robertdebock.httpd](https://galaxy.ansible.com/robertdebock/httpd/) ([source](https://github.com/robertdebock/ansible-role-httpd)).
- [robertdebock.iptables](https://galaxy.ansible.com/robertdebock/iptables/) ([source](https://github.com/robertdebock/ansible-role-iptables)).
- [robertdebock.java](https://galaxy.ansible.com/robertdebock/java/) ([source](https://github.com/robertdebock/ansible-role-java)).
- [robertdebock.ruby](https://galaxy.ansible.com/robertdebock/ruby/) ([source](https://github.com/robertdebock/ansible-role-ruby)).
- [robertdebock.scl](https://galaxy.ansible.com/robertdebock/scl/) ([source](https://github.com/robertdebock/ansible-role-scl)).
- [robertdebock.update](https://galaxy.ansible.com/robertdebock/update/) ([source](https://github.com/robertdebock/ansible-role-update)).
- [robertdebock.xinetd](https://galaxy.ansible.com/robertdebock/xinetd/) ([source](https://github.com/robertdebock/ansible-role-xinetd)).
- [robertdebock.buildtools](https://galaxy.ansible.com/robertdebock/buildtools/) ([source](https://github.com/robertdebock/ansible-role-buildtools)).
- [robertdebock.nginx](https://galaxy.ansible.com/robertdebock/nginx/) ([source](https://github.com/robertdebock/ansible-role-nginx)).
- [robertdebock.python-pip](https://galaxy.ansible.com/robertdebock/python-pip/) ([source](https://github.com/robertdebock/ansible-role-python-pip)).
- [robertdebock.postfix](https://galaxy.ansible.com/robertdebock/postfix/) ([source](https://github.com/robertdebock/ansible-role-postfix)).
- [robertdebock.rsyslog](https://galaxy.ansible.com/robertdebock/rsyslog/) ([source](https://github.com/robertdebock/ansible-role-rsyslog)).
- [robertdebock.spamassassin](https://galaxy.ansible.com/robertdebock/spamassassin/) ([source](https://github.com/robertdebock/ansible-role-spamassassin)).
- [robertdebock.docker](https://galaxy.ansible.com/robertdebock/docker/) ([source](https://github.com/robertdebock/ansible-role-docker)).
- [robertdebock.mysql](https://galaxy.ansible.com/robertdebock/mysql/) ([source](https://github.com/robertdebock/ansible-role-mysql)).
- [robertdebock.npm](https://galaxy.ansible.com/robertdebock/npm/) ([source](https://github.com/robertdebock/ansible-role-npm)).
- [robertdebock.natrouter](https://galaxy.ansible.com/robertdebock/natrouter/) ([source](https://github.com/robertdebock/ansible-role-natrouter)).
- [robertdebock.php](https://galaxy.ansible.com/robertdebock/php/) ([source](https://github.com/robertdebock/ansible-role-php)).
- [robertdebock.phpmyadmin](https://galaxy.ansible.com/robertdebock/phpmyadmin/) ([source](https://github.com/robertdebock/ansible-role-phpmyadmin)).
- [robertdebock.revealmd](https://galaxy.ansible.com/robertdebock/revealmd/) ([source](https://github.com/robertdebock/ansible-role-revealmd)).
- [robertdebock.rundeck](https://galaxy.ansible.com/robertdebock/rundeck/) ([source](https://github.com/robertdebock/ansible-role-rundeck)).
- [robertdebock.tftpd](https://galaxy.ansible.com/robertdebock/tftpd/) ([source](https://github.com/robertdebock/ansible-role-tftpd)).
- [robertdebock.tomcat](https://galaxy.ansible.com/robertdebock/tomcat/) ([source](https://github.com/robertdebock/ansible-role-tomcat)).
- [robertdebock.zabbix](https://galaxy.ansible.com/robertdebock/zabbix/) ([source](https://github.com/robertdebock/ansible-role-zabbix)).

## Dependencies
Here is an overview of the dependencies between the roles. These dependencies are set in `meta/main.yml` on each Ansible role. This means that `ansible-galaxy install` will install all required roles.

![Overview of dependencies](https://robertdebock.github.io/images/dependencies.png "Dependecy overview")

## Distributions
The goal is to let all Ansible roles work on as many distributions as possible, but this is sometimes not possible. By default these Linux distributions are included in the tests:

| Distribution | Version(s)  |
|--------------|-------------|
| Archlinux    | latest      |
| Alpine       | 3.6 & 3.7   |
| CentOS       | 6 & 7       |
| Debian       | 7, 8 & 9    |
| Fedora       | 26 & 27     |
| OpenSUSE     | 42.2 & 42.3 | 
| Ubuntu       | 14, 16 & 17 |

### Exceptions in distributions
Some Ansible roles do not work on all distributions. This table lists why.

| Ansible role | Excepted Linux distribution(s) | Reasoning |
|--------------|--------------------------------|-----------|
| robertdebock.xinetd | Alpine | The package `xinetd` is not available. |
| robertdebock.tftpd | Alpine | Depends on Ansible role robertdebock.xinetd. |
| robertdebock.tftpd | Archlinux | The package `tftpd` is not available. |
| robertdebock.python-pip | Centos 6 | Python is outdated. |
| robertdebock.rsyslog | ArchLinux | Package is only available in AUR. |
| robertdebock.spamassassin | Archlinux | Depends on Ansible role robertdebock.rsyslog. |
| robertdebock.docker | Debian 10 | Not supported by [Docker Project](https://apt.dockerproject.org/repo/dists/). |
| robertdebock.tomcat | Debian 8 & Ubuntu 14 | Java 8 is not available. |
| robertdebock.rundeck | Debian 8 & Ubuntu 14 | Java 8 is not available. |
| robertdebock.rundeck | Alpine | Package `bash` is not installed. |
| robertdebock.phpmyadmin | Centos & Ubuntu 14 | Python is outdated, PHP is outdated. |

## Ansible version
The goal is to let all roles work on these Ansible version:
```
  - ansible_version=">=2.2,<2.3"
  - ansible_version=">=2.3,<2.4"
  - ansible_version=">=2.4,<2.5"
```

### Exceptions in Ansible version
This table lists the exceptions in Ansible version and the reason why.

| Ansible role | Excepted Ansible version | Resoning |
|---|---|---|
| robertdebock.buildtools | 2.2 | DNF groups are not suppported |
| robertdebock.python-pip | 2.2 | Depends on Ansible role robertdebock.buildtools |
| robertdebock.docker | 2.2 | Depends on Ansible role robertdebock.buildtools | 
| robertdebock.release | 2.2 | A tasks uses the Ansible module `wait_for_connection` only available in 2.3 and above. |
| robertdebock.phpmyadmin | 2.2 | Got an error: `write() argument must be str, not bytes`. |
