---
title: Super important Ansible facts
---

# Super important Ansible facts

There are some facts that I use very frequently, they are super important to me.

This is more a therapeutic post for me, than it's a great read to you. ;-)

Sometimes, actually most of the times, each operating system or distribution needs something specific. For example Apache httpd has different package named for mostly every distribution. This mapping (distro:packagename) can be done using this variable: `ansible_os_family`.

Try to select packages/service-names/directories/files/etc based on the most general level and work your way down to more specific when required. This results in a sort of priority list:

1. General variable, not related to a distribution. For example: `postfix_package`.
2. `ansible_os_family` variable, related to the **type** of distribution, for example: `httpd_package` which differs for Alpine, Archlinux, Debian, Suse and RedHat. (But is't the same for docker images `debian` and `ubuntu`.)
3. `ansible_distribution` variable when each distribution has differences. For example: `reboot_requirements`. CentOS needs `yum-utils`, but Fedora needs `dnf-utils`.
4. `ansible_distribution` and `ansible_distribution_major_version` when there are differences per distribution release. For example `firewall_packages`. CentOS 6 and CentOS 7 need to have a different package.

Here is a list of containers and their `ansible_os_family`.

|Container image |ansible_os_family|
|----------------|-----------------|
|`alpine`        |`Alpine`         |
|`archlinux/base`|`Archlinux`      |
|`centos`        |`RedHat`         |
|`debian`        |`Debian`         |
|`fedora`        |`RedHat`         |
|`opensuse/leap` |`Suse`           |
|`ubuntu`        |`Debian`         |
