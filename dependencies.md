# [Dependencies](#dependencies)

Some roles need other roles to run first. The roles that run before a role are listed in its `molecule/default/prepare.yml` (and `requirements.yml`). This page orders all roles by that dependency, which is also the order in which a change that touches many roles is rolled out: a parent first, then its children.

A role is on level N when its deepest parent is on level N-1. Level 0 has no parents.

There are 171 roles on 7 levels.

## [Level 0](#level-0)

1 role.

[bootstrap](https://github.com/robertdebock/ansible-role-bootstrap)

## [Level 1](#level-1)

79 roles.

Only depending on bootstrap: [alternatives](https://github.com/robertdebock/ansible-role-alternatives), [apt_autostart](https://github.com/robertdebock/ansible-role-apt_autostart), [apt_repository](https://github.com/robertdebock/ansible-role-apt_repository), [at](https://github.com/robertdebock/ansible-role-at), [auditd](https://github.com/robertdebock/ansible-role-auditd), [auto_update](https://github.com/robertdebock/ansible-role-auto_update), [autofs](https://github.com/robertdebock/ansible-role-autofs), [awx_configuration](https://github.com/robertdebock/ansible-role-awx_configuration), [bareos_repository](https://github.com/robertdebock/ansible-role-bareos_repository), [buildtools](https://github.com/robertdebock/ansible-role-buildtools), [ca_certificates](https://github.com/robertdebock/ansible-role-ca_certificates), [common](https://github.com/robertdebock/ansible-role-common), [container_docs](https://github.com/robertdebock/ansible-role-container_docs), [core_dependencies](https://github.com/robertdebock/ansible-role-core_dependencies), [cron](https://github.com/robertdebock/ansible-role-cron), [debug](https://github.com/robertdebock/ansible-role-debug), [diskspace](https://github.com/robertdebock/ansible-role-diskspace), [dnsmasq](https://github.com/robertdebock/ansible-role-dnsmasq), [docker_compose](https://github.com/robertdebock/ansible-role-docker_compose), [dovecot](https://github.com/robertdebock/ansible-role-dovecot), [dryrun](https://github.com/robertdebock/ansible-role-dryrun), [enpass](https://github.com/robertdebock/ansible-role-enpass), [environment](https://github.com/robertdebock/ansible-role-environment), [epel](https://github.com/robertdebock/ansible-role-epel), [facts](https://github.com/robertdebock/ansible-role-facts), [filesystem](https://github.com/robertdebock/ansible-role-filesystem), [fips](https://github.com/robertdebock/ansible-role-fips), [firewall](https://github.com/robertdebock/ansible-role-firewall), [forensics](https://github.com/robertdebock/ansible-role-forensics), [functions](https://github.com/robertdebock/ansible-role-functions), [git](https://github.com/robertdebock/ansible-role-git), [gitea](https://github.com/robertdebock/ansible-role-gitea), [gitlab](https://github.com/robertdebock/ansible-role-gitlab), [gitlab_runner](https://github.com/robertdebock/ansible-role-gitlab_runner), [gnome](https://github.com/robertdebock/ansible-role-gnome), [grub](https://github.com/robertdebock/ansible-role-grub), [hostname](https://github.com/robertdebock/ansible-role-hostname), [java](https://github.com/robertdebock/ansible-role-java), [keepalived](https://github.com/robertdebock/ansible-role-keepalived), [locale](https://github.com/robertdebock/ansible-role-locale), [luks](https://github.com/robertdebock/ansible-role-luks), [lvm](https://github.com/robertdebock/ansible-role-lvm), [maintenance](https://github.com/robertdebock/ansible-role-maintenance), [memcached](https://github.com/robertdebock/ansible-role-memcached), [modprobe](https://github.com/robertdebock/ansible-role-modprobe), [mount](https://github.com/robertdebock/ansible-role-mount), [mount_options](https://github.com/robertdebock/ansible-role-mount_options), [mysql](https://github.com/robertdebock/ansible-role-mysql), [nfsserver](https://github.com/robertdebock/ansible-role-nfsserver), [npm](https://github.com/robertdebock/ansible-role-npm), [ntp](https://github.com/robertdebock/ansible-role-ntp), [openbao](https://github.com/robertdebock/ansible-role-openbao), [podman](https://github.com/robertdebock/ansible-role-podman), [powertop](https://github.com/robertdebock/ansible-role-powertop), [prosody](https://github.com/robertdebock/ansible-role-prosody), [reboot](https://github.com/robertdebock/ansible-role-reboot), [rsyslog](https://github.com/robertdebock/ansible-role-rsyslog), [scl](https://github.com/robertdebock/ansible-role-scl), [scripts](https://github.com/robertdebock/ansible-role-scripts), [selinux](https://github.com/robertdebock/ansible-role-selinux), [service](https://github.com/robertdebock/ansible-role-service), [snmpd](https://github.com/robertdebock/ansible-role-snmpd), [software](https://github.com/robertdebock/ansible-role-software), [sosreport](https://github.com/robertdebock/ansible-role-sosreport), [storage](https://github.com/robertdebock/ansible-role-storage), [subversion](https://github.com/robertdebock/ansible-role-subversion), [sysctl](https://github.com/robertdebock/ansible-role-sysctl), [sysstat](https://github.com/robertdebock/ansible-role-sysstat), [systemd](https://github.com/robertdebock/ansible-role-systemd), [tailscale](https://github.com/robertdebock/ansible-role-tailscale), [tftpd](https://github.com/robertdebock/ansible-role-tftpd), [tune2fs](https://github.com/robertdebock/ansible-role-tune2fs), [types](https://github.com/robertdebock/ansible-role-types), [ulimit](https://github.com/robertdebock/ansible-role-ulimit), [umask](https://github.com/robertdebock/ansible-role-umask), [unowned_files](https://github.com/robertdebock/ansible-role-unowned_files), [update](https://github.com/robertdebock/ansible-role-update), [upgrade](https://github.com/robertdebock/ansible-role-upgrade), [vsftpd](https://github.com/robertdebock/ansible-role-vsftpd)

## [Level 2](#level-2)

48 roles.

|Role|Depends on|
|----|----------|
|[bareos_console](https://github.com/robertdebock/ansible-role-bareos_console)|bootstrap, bareos_repository|
|[bareos_fd](https://github.com/robertdebock/ansible-role-bareos_fd)|bootstrap, bareos_repository|
|[bareos_sd](https://github.com/robertdebock/ansible-role-bareos_sd)|bootstrap, bareos_repository|
|[bareos_webui](https://github.com/robertdebock/ansible-role-bareos_webui)|bootstrap, bareos_repository|
|[cargo](https://github.com/robertdebock/ansible-role-cargo)|bootstrap, buildtools|
|[clamav](https://github.com/robertdebock/ansible-role-clamav)|bootstrap, epel|
|[cntlm](https://github.com/robertdebock/ansible-role-cntlm)|bootstrap, buildtools, epel|
|[collectd](https://github.com/robertdebock/ansible-role-collectd)|bootstrap, epel|
|[dhcpd](https://github.com/robertdebock/ansible-role-dhcpd)|bootstrap, apt_autostart, core_dependencies|
|[digitalocean_agent](https://github.com/robertdebock/ansible-role-digitalocean_agent)|bootstrap, ca_certificates, apt_autostart|
|[dns](https://github.com/robertdebock/ansible-role-dns)|bootstrap, core_dependencies|
|[dsvpn](https://github.com/robertdebock/ansible-role-dsvpn)|bootstrap, ca_certificates, core_dependencies, buildtools|
|[earlyoom](https://github.com/robertdebock/ansible-role-earlyoom)|bootstrap, buildtools|
|[eclipse](https://github.com/robertdebock/ansible-role-eclipse)|bootstrap, core_dependencies, java|
|[fail2ban](https://github.com/robertdebock/ansible-role-fail2ban)|bootstrap, epel|
|[glusterfs](https://github.com/robertdebock/ansible-role-glusterfs)|bootstrap, apt_autostart|
|[hashicorp](https://github.com/robertdebock/ansible-role-hashicorp)|bootstrap, core_dependencies|
|[haveged](https://github.com/robertdebock/ansible-role-haveged)|bootstrap, epel, sysctl|
|[investigate](https://github.com/robertdebock/ansible-role-investigate)|bootstrap, epel|
|[jenkins](https://github.com/robertdebock/ansible-role-jenkins)|bootstrap, epel, java, locale, core_dependencies|
|[jitsi](https://github.com/robertdebock/ansible-role-jitsi)|bootstrap, ca_certificates, hostname, prosody|
|[kernel](https://github.com/robertdebock/ansible-role-kernel)|bootstrap, core_dependencies, buildtools|
|[logrotate](https://github.com/robertdebock/ansible-role-logrotate)|bootstrap, cron|
|[mate](https://github.com/robertdebock/ansible-role-mate)|bootstrap, epel|
|[microsoft_repository_keys](https://github.com/robertdebock/ansible-role-microsoft_repository_keys)|bootstrap, ca_certificates|
|[mongodb](https://github.com/robertdebock/ansible-role-mongodb)|bootstrap, core_dependencies|
|[nginx](https://github.com/robertdebock/ansible-role-nginx)|bootstrap, epel|
|[obsproject](https://github.com/robertdebock/ansible-role-obsproject)|bootstrap, epel, rpmfusion|
|[openbao_agent](https://github.com/robertdebock/ansible-role-openbao_agent)|bootstrap, openbao|
|[openbao_server](https://github.com/robertdebock/ansible-role-openbao_server)|bootstrap, openbao|
|[openssh](https://github.com/robertdebock/ansible-role-openssh)|bootstrap, selinux|
|[openvpn](https://github.com/robertdebock/ansible-role-openvpn)|bootstrap, epel|
|[oracle_instant_client](https://github.com/robertdebock/ansible-role-oracle_instant_client)|bootstrap, core_dependencies|
|[postfix](https://github.com/robertdebock/ansible-role-postfix)|bootstrap, core_dependencies|
|[python_pip](https://github.com/robertdebock/ansible-role-python_pip)|bootstrap, epel, buildtools|
|[redis](https://github.com/robertdebock/ansible-role-redis)|bootstrap, epel, apt_autostart, sysctl, grub|
|[remi](https://github.com/robertdebock/ansible-role-remi)|bootstrap, epel|
|[revealmd](https://github.com/robertdebock/ansible-role-revealmd)|bootstrap, epel, git|
|[rundeck](https://github.com/robertdebock/ansible-role-rundeck)|bootstrap, java, common|
|[squid](https://github.com/robertdebock/ansible-role-squid)|bootstrap, core_dependencies|
|[swap](https://github.com/robertdebock/ansible-role-swap)|bootstrap, sysctl|
|[terraform](https://github.com/robertdebock/ansible-role-terraform)|bootstrap, core_dependencies, ca_certificates|
|[terraform_agent](https://github.com/robertdebock/ansible-role-terraform_agent)|bootstrap, ca_certificates|
|[tomcat](https://github.com/robertdebock/ansible-role-tomcat)|bootstrap, core_dependencies, java|
|[users](https://github.com/robertdebock/ansible-role-users)|bootstrap, core_dependencies|
|[virtualbox](https://github.com/robertdebock/ansible-role-virtualbox)|bootstrap, ca_certificates|
|[xrdp](https://github.com/robertdebock/ansible-role-xrdp)|bootstrap, core_dependencies, epel|
|[zabbix_repository](https://github.com/robertdebock/ansible-role-zabbix_repository)|bootstrap, ca_certificates|

## [Level 3](#level-3)

23 roles.

|Role|Depends on|
|----|----------|
|[aide](https://github.com/robertdebock/ansible-role-aide)|bootstrap, cron, postfix|
|[azure_cli](https://github.com/robertdebock/ansible-role-azure_cli)|bootstrap, epel, buildtools, python_pip, microsoft_repository_keys|
|[code](https://github.com/robertdebock/ansible-role-code)|bootstrap, ca_certificates, microsoft_repository_keys|
|[collabora_online](https://github.com/robertdebock/ansible-role-collabora_online)|bootstrap, ca_certificates, users|
|[consul](https://github.com/robertdebock/ansible-role-consul)|bootstrap, core_dependencies, hashicorp|
|[docker](https://github.com/robertdebock/ansible-role-docker)|bootstrap, core_dependencies, buildtools, epel, python_pip|
|[docker_ce](https://github.com/robertdebock/ansible-role-docker_ce)|bootstrap, epel, buildtools, python_pip, core_dependencies|
|[logwatch](https://github.com/robertdebock/ansible-role-logwatch)|bootstrap, core_dependencies, postfix|
|[mitogen](https://github.com/robertdebock/ansible-role-mitogen)|bootstrap, core_dependencies, epel, python_pip|
|[molecule](https://github.com/robertdebock/ansible-role-molecule)|bootstrap, buildtools, epel, python_pip|
|[nomad](https://github.com/robertdebock/ansible-role-nomad)|bootstrap, core_dependencies, hashicorp|
|[openssl](https://github.com/robertdebock/ansible-role-openssl)|bootstrap, buildtools, epel, python_pip|
|[postgres](https://github.com/robertdebock/ansible-role-postgres)|bootstrap, buildtools, epel, python_pip|
|[spamassassin](https://github.com/robertdebock/ansible-role-spamassassin)|bootstrap, core_dependencies, cron, logrotate, rsyslog|
|[tigervnc](https://github.com/robertdebock/ansible-role-tigervnc)|bootstrap, core_dependencies, gnome, users|
|[update_pip_packages](https://github.com/robertdebock/ansible-role-update_pip_packages)|bootstrap, epel, buildtools, python_pip|
|[vagrant](https://github.com/robertdebock/ansible-role-vagrant)|bootstrap, core_dependencies, buildtools, epel, python_pip|
|[vault](https://github.com/robertdebock/ansible-role-vault)|bootstrap, core_dependencies, hashicorp|
|[vault_agent](https://github.com/robertdebock/ansible-role-vault_agent)|bootstrap, core_dependencies, hashicorp|
|[vault_oracle_plugin](https://github.com/robertdebock/ansible-role-vault_oracle_plugin)|bootstrap, core_dependencies, oracle_instant_client|
|[zabbix_agent](https://github.com/robertdebock/ansible-role-zabbix_agent)|bootstrap, ca_certificates, zabbix_repository|
|[zabbix_proxy](https://github.com/robertdebock/ansible-role-zabbix_proxy)|bootstrap, ca_certificates, zabbix_repository|
|[zabbix_server](https://github.com/robertdebock/ansible-role-zabbix_server)|bootstrap, selinux, container_docs, mysql, ca_certificates, zabbix_repository, core_dependencies|

## [Level 4](#level-4)

9 roles.

|Role|Depends on|
|----|----------|
|[artifactory](https://github.com/robertdebock/ansible-role-artifactory)|bootstrap, core_dependencies, java, postgres|
|[backup](https://github.com/robertdebock/ansible-role-backup)|bootstrap, mysql, buildtools, epel, python_pip, postgres|
|[bareos_dir](https://github.com/robertdebock/ansible-role-bareos_dir)|bootstrap, buildtools, postgres, core_dependencies, postfix, bareos_repository|
|[harbor](https://github.com/robertdebock/ansible-role-harbor)|bootstrap, core_dependencies, buildtools, epel, python_pip, docker, docker_compose, selinux|
|[httpd](https://github.com/robertdebock/ansible-role-httpd)|bootstrap, epel, buildtools, python_pip, openssl|
|[restore](https://github.com/robertdebock/ansible-role-restore)|bootstrap, core_dependencies, mysql, buildtools, epel, python_pip, postgres|
|[vault_auditdevice](https://github.com/robertdebock/ansible-role-vault_auditdevice)|bootstrap, core_dependencies, hashicorp, vault|
|[vault_autosnapshot](https://github.com/robertdebock/ansible-role-vault_autosnapshot)|bootstrap, core_dependencies, hashicorp, vault|
|[vault_configuration](https://github.com/robertdebock/ansible-role-vault_configuration)|bootstrap, core_dependencies, hashicorp, vault|

## [Level 5](#level-5)

6 roles.

|Role|Depends on|
|----|----------|
|[ca](https://github.com/robertdebock/ansible-role-ca)|bootstrap, buildtools, epel, python_pip, openssl, httpd|
|[certbot](https://github.com/robertdebock/ansible-role-certbot)|bootstrap, cron, buildtools, epel, python_pip, openssl, selinux, httpd|
|[haproxy](https://github.com/robertdebock/ansible-role-haproxy)|bootstrap, core_dependencies, epel, buildtools, python_pip, openssl, httpd|
|[php](https://github.com/robertdebock/ansible-role-php)|bootstrap, epel, python_pip, buildtools, openssl, httpd|
|[php_fpm](https://github.com/robertdebock/ansible-role-php_fpm)|bootstrap, epel, buildtools, python_pip, openssl, httpd|
|[vault_initialize](https://github.com/robertdebock/ansible-role-vault_initialize)|bootstrap, core_dependencies, hashicorp, vault, vault_configuration|

## [Level 6](#level-6)

5 roles.

|Role|Depends on|
|----|----------|
|[mediawiki](https://github.com/robertdebock/ansible-role-mediawiki)|bootstrap, core_dependencies, epel, python_pip, buildtools, openssl, httpd, php, mysql|
|[nextcloud](https://github.com/robertdebock/ansible-role-nextcloud)|bootstrap, core_dependencies, cron, buildtools, epel, python_pip, openssl, selinux, httpd, redis, php, php_fpm, mysql|
|[phpmyadmin](https://github.com/robertdebock/ansible-role-phpmyadmin)|bootstrap, core_dependencies, buildtools, epel, python_pip, openssl, selinux, httpd, mysql, php|
|[roundcubemail](https://github.com/robertdebock/ansible-role-roundcubemail)|bootstrap, epel, buildtools, python_pip, openssl, selinux, httpd, php, mysql|
|[zabbix_web](https://github.com/robertdebock/ansible-role-zabbix_web)|bootstrap, selinux, container_docs, buildtools, epel, python_pip, openssl, mysql, php, httpd, ca_certificates, zabbix_repository, core_dependencies, zabbix_server|

