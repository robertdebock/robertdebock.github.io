# [Distribution relations](#distribution-relations)

There is a relation between:

- Ansilbe Platforms & Versions in `meta/main.yml` for a role.
- Docker Hub image names & Tags.
- GitHub Branches.

Because these 3 relations can be confusing, here is a table that explains the relation.

|Ansible Platform |Platform Version|Image      |Tag    |Branch  |
|-----------------|----------------|-----------|-------|--------|
|Amazon           |**not used**    |amazonlinux|1      |1       |
|Amazon           |2018.30         |amazonlinux|latest |master  |
|Alpine           |**all**         |alpine     |latest |master  |
|Alpine           |**all**         |alpine     |edge   |edge    |
|**EL**           |7               |centos     |7      |7       |
|**EL**           |8               |centos     |latest |master  |
|**EL**           |7               |oraclelinux|7      |7       |
|**EL**           |8               |oraclelinux|latest |master  |
|**EL**           |7               |redhat     |7      |7       |
|**EL**           |8               |redhat     |latest |master  |
|Debian           |**all**         |debian     |latest |master  |
|Debian           |**buster**      |debian     |latest |master  |
|Debian           |**not used**    |debian     |latest |testing |
|Debian           |**not used**    |debian     |latest |unstable|
|Fedora           |32              |fedora     |32     |32      |
|Fedora           |33              |fedora     |latest |master  |
|Fedora           |rawhide         |fedora     |rawhide|rawhide |
|OpenSUSE         |all             |opensuse   |latest |master  |
|Ubuntu           |**all**         |ubuntu     |latest |master  |
|Ubuntu           |**bionic**      |ubuntu     |latest |master  |

All **bold** printed items require some kind of attention. This could mean:

- A single Ansible Galaxy Platform Version always tests on multiple Docker Hub Tags. (Alpine - all)
- A single Ansible Galaxy Platform refers to multiple Docker Hub Images. (EL)
- Multiple Ansible Galaxy Platform Versions refer to the same Docker Hub Image Tag. (Debian & Ubuntu)
