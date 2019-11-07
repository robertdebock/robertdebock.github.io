---
title: Relations between containernames, setup and Galaxy
---

# Relations between containernames, setup and Galaxy

It's not easy to find the relation between container names, facts returned from setup (or `gather_facts`) and Ansible Galaxy platform names.

Here is an attempt to make life a little easier:

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
|fedora         |Fedora              |RedHat           |30                                |Fedora         |30            |
|fedora:rawhide |Fedora              |RedHat           |32                                |Fedora         |all           |
|opensuse       |OpenSUSE            |Suse             |15                                |opensuse       |all           |
|oraclelinux:7  |OracleLinux         |RedHat           |7                                 |EL             |7             |
|oraclelinux    |OracleLinux         |RedHat           |8                                 |EL             |7             |
|ubuntu         |Ubuntu              |Debian           |18                                |Ubuntu         |bionic        |
