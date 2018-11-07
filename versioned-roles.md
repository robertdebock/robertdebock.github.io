## Versioned Ansible roles
Some roles have a reference to a version in them, which means the role determines what verion of that software is installed.

These roles require a bit more attention; every now and then the upstream released version should be cheched.

Here are roles with a version in them and the webpage of the upstream project.

| role | upstream |
|------|----------|
|[artifactory](https://github.com/robertdebock/ansible-role-artifactory/blob/master/defaults/main.yml)|[download page](https://dl.bintray.com/jfrog/artifactory/)|
|[awx](https://github.com/robertdebock/ansible-role-awx/blob/master/defaults/main.yml)|[github release](https://github.com/ansible/awx/releases)|
|[cntlm](https://github.com/robertdebock/ansible-role-cntlm/blob/master/defaults/main.yml)|[sourceforge](https://sourceforge.net/projects/cntlm/files/)|
|[irslackd](https://github.com/robertdebock/ansible-role-irslackd/blob/master/defaults/main.yml)|[github release](https://github.com/adsr/irslackd/releases)|
|[java](https://github.com/robertdebock/ansible-role-java/blob/master/vars/main.yml)|[download page](https://www.oracle.com/technetwork/java/javaseproducts/downloads/index.html)|
|[mediawiki](https://github.com/robertdebock/ansible-role-mediawiki/blob/master/defaults/main.yml)|[download page](https://www.mediawiki.org/wiki/Download)|
|[mssql](https://github.com/robertdebock/ansible-role-mssql/blob/master/vars/main.yml)|[rhel repository](https://packages.microsoft.com/rhel/7/mssql-server-2017/), [ubuntu repository](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017/pool/main/m/mssql-server/) & [opensuse repository](https://packages.microsoft.com/sles/12/mssql-server-2017/)|
|[packer](https://github.com/robertdebock/ansible-role-packer/blob/master/defaults/main.yml)|[front page](https://www.packer.io/)|
|[phpmyadmin](https://github.com/robertdebock/ansible-role-phpmyadmin/blob/master/defaults/main.yml)|[download page](https://www.phpmyadmin.net/downloads/)|
|[rundeck](https://github.com/robertdebock/ansible-role-rundeck/blob/master/vars/main.yml)|[download page](https://rundeck.org/downloads.html)|
|[snort](https://github.com/robertdebock/ansible-role-snort/blob/master/vars/main.yml)|[download page](https://www.snort.org/downloads)|
|[sudo_pair](https://github.com/robertdebock/ansible-role-sudo-pair/blob/master/defaults/main.yml)|[github release](https://github.com/square/sudo_pair/releases)|
|[terraform](https://github.com/robertdebock/ansible-role-terraform/blob/master/defaults/main.yml)|[download page](https://www.terraform.io/downloads.html)|
|[tomcat](https://github.com/robertdebock/ansible-role-tomcat/blob/master/defaults/main.yml)|[7 branch](https://tomcat.apache.org/download-70.cgi), [8 branch](https://tomcat.apache.org/download-80.cgi) and the [9 branch](https://tomcat.apache.org/download-90.cgi)|
|[zabbix](https://github.com/robertdebock/ansible-role-zabbix/blob/master/defaults/main.yml)|[download page](https://www.zabbix.com/download)|
