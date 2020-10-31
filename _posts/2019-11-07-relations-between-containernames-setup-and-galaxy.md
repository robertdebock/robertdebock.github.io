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
|all           |edge      |?                                 |

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

## Archlinux

```
containername: archlinux
ansible_os_family: Archlinux
ansible_distribution: Archlinux
galaxy_platform: ArchLinux
```

|galaxy_version|docker_tag|ansible_distribution_major_version|
|--------------|----------|----------------------------------|
|all           |latest    |?                                 |

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
|buster        |latest    |buster                            |
|bullseye      |?         |?                                 |


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
|bionic        |latest    |18                                |

|containername  |ansible_distribution|ansible_os_family|ansible_distribution_major_version|galaxy_platform|galaxy_version|
|---------------|--------------------|-----------------|----------------------------------|---------------|--------------|
|alpine         |Alpine              |Alpine           |3                                 |Alpine         |all           |
|alpine:edge    |Alpine              |Alpine           |?                                 |Alpine         |all           |
|amazonlinux:1  |Amazon              |RedHat           |2018                              |Amazon         |2018.03       |
|amazonlinux    |Amazon              |RedHat           |2                                 |Amazon         |Candidate     |
|archlinux      |Archlinux           |Archlinux        |?                                 |ArchLinux      |all           |
|centos:7       |CentOS              |EL               |7                                 |EL             |7             |
|centos         |CentOS              |EL               |8                                 |EL             |8             |
|debian         |Debian              |Debian           |buster                            |Debian         |buster        |
|debian:unstable|Debian              |Debian           |sid                               |Debian         |sid           |
|fedora         |Fedora              |RedHat           |33                                |Fedora         |33            |
|fedora:32      |Fedora              |RedHat           |32                                |Fedora         |32            |
|fedora:rawhide |Fedora              |RedHat           |34                                |Fedora         |34            |
|opensuse       |OpenSUSE            |Suse             |15                                |opensuse       |all           |
|ubuntu         |Ubuntu              |Debian           |18                                |Ubuntu         |bionic        |
