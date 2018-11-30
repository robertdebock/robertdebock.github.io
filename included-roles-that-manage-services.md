## Included roles that manage services

For the roles depending on other roles managing a service, molecule has to be configured to **not** manage the service in Docker by setting `{{ role_name}}_ignore_docker` to `no`. Docker does not allow managing services on most Docker images.

## Roles that include other roles

These role depend on a role that starts a service:

| role          | dependencies          |
|---------------|-----------------------|
| owncloud      | httpd, php_fpm, redis |
| mediawiki     | httpd                 |
| openvas       | redis                 |
| phpmyadmin    | httpd, mysql          |
| php           | httpd                 |
| roundcubemail | httpd                 |
| spamassassin  | rsyslog               |
| tftpd         | xinetd                |

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
