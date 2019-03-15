hard dependecies" on another role, mainy for a shared handler or variables set in the parent role, used in the child role. More details on [how to use these roles](how-to-use-these-roles.html). These hard dependencies are describe in `meta/main.yml` under `dependencies`.

| Role          | Depends on | Reason |
|---------------|------------|--------|
| [tftpd](https://galaxy.ansible.com/robertdebock/tftpd/) | [xinetd](https://galaxy.ansible.com/robertdebock/xinetd/) | handler |
| [php](https://galaxy.ansible.com/robertdebock/php/) | [httpd](https://galaxy.ansible.com/robertdebock/httpd/) | handler |
| [phpmyadmin](https://galaxy.ansible.com/robertdebock/phpmyadmin/) | [httpd](https://galaxy.ansible.com/robertdebock/httpd/) | handler |
| [roundcubemail](https://galaxy.ansible.com/robertdebock/roundcubemail/) | [httpd](https://galaxy.ansible.com/robertdebock/httpd/) | handler |
| [spamassassin](https://galaxy.ansible.com/robertdebock/spamassassin/) | [rsyslog](https://galaxy.ansible.com/robertdebock/rsyslog/) | handler |
| [zabbix](https://galaxy.ansible.com/robertdebock/zabbix/) | [httpd](https://galaxy.ansible.com/robertdebock/httpd/) | handler & inherited variable |
| [ca](https://galaxy.ansible.com/robertdebock/ca/) | [httpd](https://galaxy.ansible.com/robertdebock/httpd/) | inherited variable |
| [common](https://galaxy.ansible.com/robertdebock/common/) | [reboot](https://galaxy.ansible.com/robertdebock/reboot/) | A reboot is used in `tasks/main.yml` with `include_role`. |
| [openvas](https://galaxy.ansible.com/robertdebock/openvas/) | [selinux](https://galaxy.ansible.com/robertdebock/selinux/) | SELinux is configured in `tasks/main.yml` with `include_role`. |
| [openvpn](https://galaxy.ansible.com/robertdebock/openvpn/) | [ca](https://galaxy.ansible.com/robertdebock/ca/) | OpenSSL keys are created in `tasks/main.yml` with `include_role`. |
| **[roundcubemail](https://galaxy.ansible.com/robertdebock/roundcubemail/)** | [php](https://galaxy.ansible.com/robertdebock/php/) | Specific PHP settigs are required. |
| [selinux](https://galaxy.ansible.com/robertdebock/selinux/) | [reboot](https://galaxy.ansible.com/robertdebock/reboot/) | A reboot is used in `tasks/main.yml` with `include_role`. |
| [update](https://galaxy.ansible.com/robertdebock/update/) | [reboot](https://galaxy.ansible.com/robertdebock/reboot/) | A reboot is used in `tasks/main.yml` with `include_role`. |

## Tests
Unit tests and integration tests are use to verify the quality of the roles, read [more about testing](testing.html)

## Distributions
The goal is to let all Ansible roles work on as many distributions as possible, but this is sometimes not possible. For each distribution, the current and previous release is tested. A role may work on diferent distributions, like Red Hat Enterprise Linux (RHEL), but it's not tested against it. By default these Linux distributions are included in the tests:

| Distribution | Version(s)                  |
|--------------|-----------------------------|
| Archlinux    | latest                      |
| Alpine       | latest & edge*              |
| CentOS       | 6 & latest                  |
| Debian       | stable, latest & unstable*  |
| Fedora       | latest & rawhide*           |
| OpenSUSE     | leap & tumbleweed           | 
| Ubuntu       | 17 (artful), latest, devel* |

* = These are experimental, builds are done for informative purposes and may fail.

### Exceptions in distributions
Some Ansible roles do not work on all distributions. This table lists why.

| Ansible role | Excepted Linux distribution(s) | Reasoning |
|--------------|--------------------------------|-----------|
| [ara](https://galaxy.ansible.com/robertdebock/ara) | CentOS 6 | Depends on Ansible role [python_pip](https://galaxy.ansible.com/robertdebock/python_pip/). |
| [awx](https://galaxy.ansible.com/robertdebock/ara) | CentOS 6 | Depends on Ansible role [python_pip](https://galaxy.ansible.com/robertdebock/python_pip/). |
| [cargo](https://galaxy.ansible.com/robertdebock/cargo) | Alpine & CentOS 6 | Weird error & rust is too old. |
| [cloud9](https://galaxy.ansible.com/robertdebock/cloud9) | Alpine | `npm: exec: line 2: /home/cloud9/.c9/node/bin/node: not found`. |
| [cloud9](https://galaxy.ansible.com/robertdebock/cloud9) | ArchLinux, CentOS 6 | `Python version 2.7 is required to install pty.js.`. |
| [cloud9](https://galaxy.ansible.com/robertdebock/cloud9) | OpenSUSE | `configure: error: "curses not found"`. |
| [digitalocean-agent](https://galaxy.ansible.com/robertdebock/dititalocean_agent) | Alpine, ArchLinux, OpenSUSE  | Not supported by [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-the-digitalocean-agent-for-monitoring). |
| [digitalocean-agent](https://galaxy.ansible.com/robertdebock/dititalocean_agent) | Debian, Ubuntu | Package attempts to start service which is not possible in Docker. |
| [docker](https://galaxy.ansible.com/robertdebock/docker) | CentOS 6 | Depends on Ansible role [python_pip](https://galaxy.ansible.com/robertdebock/python_pip/). |
| [docker_ce](https://galaxy.ansible.com/robertdebock/docker_ce) | Many | Docker CE has [limited support for distributions](https://docs.docker.com/install/#supported-platforms). |
| [glusterfs](https://galaxy.ansible.com/robertdebock/glusterfs) | Alpine | GlusterFS is [not available](https://github.com/gluster/glusterfs/issues/268). | 
| [httpd](https://galaxy.ansible.com/robertdebock/httpd) | CentOS 6 | `the SNI (Subject Name Indication) extension to TLS is not available on this platform`. | 
| [mitogen](https://galaxy.ansible.com/robertdebock/mitogen) | CentOS 6 | Depends on Ansible role [python_pip](https://galaxy.ansible.com/robertdebock/python_pip/). |
| [mssql](https://galaxy.ansible.com/robertdebock/mssql) | Alpine, ArchLinux, CentOS 6, Debian, Fedora, OpenSUSE Tumbleweed & Ubuntu latest | [Not supported](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup?view=sql-server-linux-2017) by Microsoft. |
| [npm](https://galaxy.ansible.com/robertdebock/npm) | OpenSUSE Tumbleweed | `No JSON object could be decoded`. |
| [openvas](https://galaxy.ansible.com/robertdebock/openvas) | Archlinux, CentOS 6 | Depends on Ansible role [python_pip](https://galaxy.ansible.com/robertdebock/python_pip/) and [selinux](https://galaxy.ansible.com/robertdebock/selinux/). |
| [owncloud](https://galaxy.ansible.com/robertdebock/owncloud) | CentOS 6 | Depends on Ansible role [python_pip](https://galaxy.ansible.com/robertdebock/python_pip/). |
| [php](https://galaxy.ansible.com/robertdebock/php) | CentOS 6 | Depends on Ansible role [httpd](https://galaxy.ansible.com/robertdebock/httpd/). |
| [phpmyadmin](https://galaxy.ansible.com/robertdebock/phpmyadmin) | CentOS 6 | Depends on Ansible role [httpd](https://galaxy.ansible.com/robertdebock/httpd/). |
| [phpmyadmin](https://galaxy.ansible.com/robertdebock/phpmyadmin) | Alpine | There is no MySQL, only mariadb. |
| [python_pip](https://galaxy.ansible.com/robertdebock/python_pip) | CentOS 6 | `No package matching 'python-pip' found available`. |
| [redis](https://galaxy.ansible.com/robertdebock/redis) | CentOS 6 | Depends on Ansible role [python_pip](https://galaxy.ansible.com/robertdebock/python_pip/). |
| [revealmd](https://galaxy.ansible.com/robertdebock/revealmd) | OpenSUSE Tumbleweed | `No JSON object could be decoded`. |
| [roundcubemail](https://galaxy.ansible.com/robertdebock/roundcubemail) | CentOS 6 | Depends on Ansible role [httpd](https://galaxy.ansible.com/robertdebock/httpd/). |
| [rsyslog](https://galaxy.ansible.com/robertdebock/rsyslog) | ArchLinux | Package is only available in AUR. |
| [selinux](https://galaxy.ansible.com/robertdebock/selinux) | ArchLinux | Package is only available in AUR. |
| [spamassassin](https://galaxy.ansible.com/robertdebock/spamassassin) | Archlinux | Depends on Ansible role [rsyslog](https://galaxy.ansible.com/robertdebock/rsyslog/). |
| [sudo_pair](https://galaxy.ansible.com/robertdebock/sudo_pair) | Alpine & CentOS 6 | Depends on Ansible role [cargo](https://galaxy.ansible.com/robertdebock/cargo/). |
| [tftpd](https://galaxy.ansible.com/robertdebock/tftpd) | Alpine | Depends on Ansible role [xinetd](https://galaxy.ansible.com/robertdebock/xinetd/). |
| [tftpd](https://galaxy.ansible.com/robertdebock/tftpd) | Archlinux | The package `tftpd` is not available. |
| [xinetd](https://galaxy.ansible.com/robertdebock/xinetd) | Alpine | The package `xinetd` is not available. |
| [zabbix](https://galaxy.ansible.com/robertdebock/zabbix) | ArchLinux, Alpine, Debian, Fedora & OpenSUSE | Zabbix has [limited OS support](https://www.zabbix.com/documentation/3.4/manual/installation/requirements). |

## Ansible version
The goal is to let all roles work on these Ansible version:
- 2.4
- 2.5
- 2.6
- 2.7
- devel (which may fail)

See errors? Please help and [make a merge request on git](https://github.com/robertdebock/robertdebock.github.io/).
