# [Distribution relations](#distribution-relations)

There is a relation between:

- Ansible Platforms & Versions in `meta/main.yml` for a role.
- Docker Hub image names & Tags. (The images are `robertdebock/<image>:<tag>`.)
- The branches of the `docker-<distribution>-systemd` repositories on GitHub, which build and push these images.

Because these 3 relations can be confusing, here is a table that explains the relation.

|Ansible Platform |Platform Version|Image          |Tag     |Branch  |
|-----------------|----------------|---------------|--------|--------|
|Alpine           |**all**         |alpine         |latest  |master  |
|Amazon           |Candidate       |amazonlinux    |latest  |master  |
|EL               |9               |enterpriselinux|latest  |master  |
|EL               |10              |**not used**   |        |        |
|Debian           |**all**         |debian         |latest  |master  |
|Debian           |trixie (13)     |debian         |latest  |master  |
|Debian           |bookworm (12)   |debian         |bookworm|bookworm|
|Fedora           |44              |fedora         |44      |44      |
|Fedora           |45              |fedora         |latest  |master  |
|Ubuntu           |**all**         |ubuntu         |latest  |master  |
|Ubuntu           |noble (24.04)   |ubuntu         |latest  |master  |
|Ubuntu           |jammy (22.04)   |ubuntu         |jammy   |jammy   |

All **bold** printed items require some kind of attention. This could mean:

- A single Ansible Galaxy Platform Version always tests on multiple Docker Hub Tags. (Alpine, Debian and Ubuntu - all)
- Multiple Ansible Galaxy Platform Versions refer to the same Docker Hub Image Tag. (Debian & Ubuntu)
- There is no image for this platform version yet, so it is not tested. (EL 10)
