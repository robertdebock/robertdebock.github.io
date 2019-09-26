---
title: How many modules do you need?
---

# How many modules do you need?

[Ansible Collections](https://docs.ansible.com/ansible/devel/user_guide/collections_using.html) are coming. A big change in Ansible, so a stable version will likely be a good moment to go to Ansible 3. Listening to the developers, I think we can expect Ansible 3 in the spring of 2020.

Anyway, let's get some stats:

# How many modules am I using?

Thas was not so dificult to estimate: **97 modules**.

# What 'weird' modules?

A bit more difficult to answer, I've taken two approaches:
- Take the bottom of the list of "most used modules".
- Walked throught the 97 modules and discover odd once.

# How many 'vendor' modules?

I expect some Ansible Collections will be maintained by the vendors; Google (GCP), Microsoft (Azure), F5 (BigIP), yum (RedHat), etc. That's why knowing this upfront is likely smart.

|Module              |Times used   |Potential maintainer |
|--------------------|-------------|---------------------|
|pip                 |17           |PyPi                 |
|apt                 |16           |Canonical            |
|yum                 |9            |Red Hat              |
|apt_key             |6            |Canonical            |
|apt_repository      |5            |Canonical            |
|rpm_key             |4            |Red Hat              |
|zypper              |3            |SUSE                 |
|yum_repository      |3            |Red Hat              |
|dnf                 |3            |Red Hat/Fedora       |
|zypper_repository   |2            |SUSE                 |
|zabbix_host         |2            |Zabbix               |
|zabbix_group        |2            |Zabbix               |
|apk                 |2            |Alpine               |
|tower_*             |7 (combined) |RedHat               |
|redhat_subscription |1            |RedHat               |
|pacman              |1            |ArchLinux            |
|bigip_*             |6 (combined) |F5                   |

# How often do I use what modules?

|Place |Module     | Times used |
|------|-----------|------------|
|1     |package    |138         |
|1     |service    |137         |
|1     |command    |73          |
|1     |template   |64          |
|1     |file       |62          |
|1     |meta       |27          |
|1     |assert     |26          |
|1     |unarchive  |24          |
|1     |lineinfile |21          |
|1     |copy       |20          |

Wow, I'm especially surprised by two modules:
1. command - I'm going to review if there are modules that I can use instead of `command`. I know very well that command should be used as a last resort, not 73 times... Painful.
2. assert - Most roles used to see of variable met the criteria. (If a variable is defined and the type is correct.) Rather wait for [`role spec`](https://github.com/ansible/proposals/issues/39).
