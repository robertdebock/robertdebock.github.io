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

## 2. Run molecule in the dind container.

```shell
docker run -ti -v /data:/github/workspace/ansible-role-example -v /run/docker.sock:/run/docker.sock robertdebock/github-action-molecule /bin/bash
```
