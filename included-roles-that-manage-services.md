## Included roles that manage services

For the roles depending on other roles managing a service, molecule has to be configured to **not** manage the service in Docker by setting `{{ role_name}}_ignore_docker` to `no`. Docker does not allow managing services on most Docker images.

## Roles that include other roles

These role depend on a role that starts a service:

| role          | dependencies          |
|---------------|-----------------------|
| [owncloud](https://galaxy.ansible.com/robertdebock/owncloud) | [httpd](https://galaxy.ansible.com/robertdebock/httpd), [php_fpm](https://galaxy.ansible.com/robertdebock/php_fpm) & [redis](https://galaxy.ansible.com/robertdebock/redis) |
| [mediawiki](https://galaxy.ansible.com/robertdebock/mediawiki) | [httpd](https://galaxy.ansible.com/robertdebock/httpd) |
| [openvas](https://galaxy.ansible.com/robertdebock/openvas) | [redis](https://galaxy.ansible.com/robertdebock/redis) |
| [phpmyadmin](https://galaxy.ansible.com/robertdebock/phpmyadmin) | [httpd](https://galaxy.ansible.com/robertdebock/httpd) & [mysql](https://galaxy.ansible.com/robertdebock/mysql) |
| [php](https://galaxy.ansible.com/robertdebock/php) | [httpd](https://galaxy.ansible.com/robertdebock/httpd) |
| [roundcubemail](https://galaxy.ansible.com/robertdebock/roundcubemail) | [httpd](https://galaxy.ansible.com/robertdebock/httpd) |
| [spamassassin](https://galaxy.ansible.com/robertdebock/spamassassin) | [rsyslog](https://galaxy.ansible.com/robertdebock/rsyslog) |
| [tftpd](https://galaxy.ansible.com/robertdebock/tftpd) | [xinetd](https://galaxy.ansible.com/robertdebock/xinetd) |

This list can be (re-)generated using:

```
grep -E "($(grep -Ril 'service:' ansible-role-* | cut -d/ -f1 | cut -d- -f3 | sort | uniq | xargs | sed 's/ /|/g'))" */requirements.yml
```

## Example

For example owncloud need extra variables to not start the depending services.

```yaml
provisioner:
  name: ansible
  inventory:
    group_vars:
      all:
        httpd_ignore_docker: no
        redis_ignore_docker: no
        php_fpm_ignore_docker: no
```
