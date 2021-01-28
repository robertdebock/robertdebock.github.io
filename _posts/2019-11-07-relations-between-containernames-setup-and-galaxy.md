---
title: Relations between containernames, setup and Galaxy
---

# Relations between containernames, setup and Galaxy

It's not easy to find the relation between container names, facts returned from setup (or `gather_facts`) and Ansible Galaxy platform names.

Here is an attempt to make life a little easier:

## Alpine

```
containername: alpine
ansible_os_family: Alpine
ansible_distribution: Alpine
galaxy_platform: Alpine
```

|galaxy_version|docker_tag|ansible_distribution_major_version|
|--------------|----------|----------------------------------|
|all           |latest    |3                                 |
|all           |edge      |3                                 |

## AmazonLinux

```
containername: amazonlinux
ansible_os_family: RedHat
ansible_distribution: Amazon
galaxy_platform: Amazon
```

|galaxy_version|docker_tag|ansible_distribution_major_version|
|--------------|----------|----------------------------------|
|Candidate     |latest    |2                                 |
|2018.03       |1         |2018                              |

## CentOS

```
containername: centos
ansible_os_family: RedHat
ansible_distribution: CentOS
galaxy_platform: EL
```

|galaxy_version|docker_tag|ansible_distribution_major_version|
|--------------|----------|----------------------------------|
|8             |latest    |8                                 |
|7             |7         |7                                 |

## Debian

```
containername: debian
ansible_os_family: Debian
ansible_distribution: Debian
galaxy_platform: Debian
```

|galaxy_version|docker_tag|ansible_distribution_major_version|
|--------------|----------|----------------------------------|
|buster        |latest    |10                                |
|bullseye      |bullseye  |testing                           |

## Fedora

```
containername: fedora
ansible_os_family: RedHat
ansible_distribution: Fedora
galaxy_platform: Fedora
```

|galaxy_version|docker_tag|ansible_distribution_major_version|
|--------------|----------|----------------------------------|
|32            |32        |32                                |
|33            |latest    |33                                |
|34            |rawhide   |34                                |

## OpenSUSE

```
containername: opensuse
ansible_os_family: Suse
ansible_distribution: OpenSUSE
galaxy_platform: opensuse
```

|galaxy_version|docker_tag|ansible_distribution_major_version|
|--------------|----------|----------------------------------|
|all           |latest    |15                                |

## Ubuntu

```
containername: ubuntu
ansible_os_family: Debian
ansible_distribution: Ubuntu
galaxy_platform: Ubuntu
```

|galaxy_version|docker_tag|ansible_distribution_major_version|
|--------------|----------|----------------------------------|
|focal         |latest    |20                                |
|bionic        |bionic    |18                                |
|xenial        |xenial    |16                                |
