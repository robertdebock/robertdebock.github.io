## Versioned Ansible roles
Some roles have a reference to a version in them, which means the role determines what verion of that software is installed.

These roles require a bit more attention; every now and then the upstream released version should be cheched.

Here are roles with a version in them and the webpage of the upstream project.

| role | upstream |
|------|----------|
|[rundeck](https://github.com/robertdebock/ansible-role-rundeck/blob/master/vars/main.yml)|https://rundeck.org/downloads.html|
|[java](https://github.com/robertdebock/ansible-role-java/blob/master/vars/main.yml)|https://www.oracle.com/technetwork/java/javaseproducts/downloads/index.html|
|[mediawiki](https://github.com/robertdebock/ansible-role-mediawiki/blob/master/defaults/main.yml)|https://www.mediawiki.org/wiki/Download|
|[zabbix](https://github.com/robertdebock/ansible-role-zabbix/blob/master/defaults/main.yml)|https://www.zabbix.com/download|
|[cntlm](https://github.com/robertdebock/ansible-role-cntlm/blob/master/defaults/main.yml)|https://sourceforge.net/projects/cntlm/files/|
|[terraform](https://github.com/robertdebock/ansible-role-terraform/blob/master/defaults/main.yml)|https://www.terraform.io/downloads.html|
|[packer](https://github.com/robertdebock/ansible-role-packer/blob/master/defaults/main.yml)|https://www.packer.io/|
|[tomcat]()|x|
|[artifactory]()|x|
|[awx]()|x|
|[sudo_pair]()|x|
|[irslackd]()|x|
|[phpmyadmin]()|x|
