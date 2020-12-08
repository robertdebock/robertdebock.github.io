---
title: Debugging GitLab builds
---

# Debugging GitLab builds

Now that [Travis has become unusable](https://blog.travis-ci.com/2020-11-02-travis-ci-new-billing), I'm moving stuff to [GitLab](https://gitlab.com/robertdebock). Some builds are breaking, this is how to reproduce the errors.

## 1. Start the Docker in Docker (dind) container.

From the directory of your role, for example `/opt/ansible-role-example`, run:

```shell
docker run -ti --privileged --name dind -d -v $(pwd):/data docker:dind
```

## 2. Login to the dind container.

```shell
docker exec -ti dind /bin/sh
```

You're now in the `dind` container that GitLab also uses.

## 3. Start the molecule container.

```shell
docker run -ti -v /data:/github/workspace/ansible-role-example -v /run/docker.sock:/run/docker.sock -v /sys/fs/cgroup:/sys/fs/cgroup:ro robertdebock/github-action-molecule /bin/bash
```

## 4. Run molecule.

```shell
cd ansible-role-example
molecule test --destroy=never
molecule login
```

Now you're ready to run commands and troubleshoot.
