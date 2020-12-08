---
title: Debugging GitLab builds
---

# Debugging GitLab builds

Now that [Travis has become unusable](https://blog.travis-ci.com/2020-11-02-travis-ci-new-billing), I'm moving stuff to [GitLab](https://gitlab.com/robertdebock). Some builds are breaking, this is how to reproduce the errors.

## Start the `dind` container

```shell
docker run --name gitlabci --volume $(pwd)/ansible-role-dns:/ansible-role-dns:z --privileged --tty --interactive docker:stable-dind
```

## Login to the `dind` container

```shell
docker exec --tty --interactive gitlabci /bin/sh
```

## Install software

The dind image is Alpine based and misses required software to run `molecule` or `tox`.

```shell
apk add --no-cache python3 python3-dev py3-pip gcc git curl build-base autoconf automake py3-cryptography linux-headers musl-dev libffi-dev openssl-dev openssh
```

## Tox

GitLab CI tries to run tox (if `tox.ini` is found). To emulate GitLab CI, run:

```shell
python3 -m pip install tox --ignore-installed
```

And simply run `tox` to see the results.

```shell
tox
```

## Molecule

For more in-depth troubleshooting, try installing molecule:

```
python3 -m pip install ansible molecule[docker] docker ansible-lint
```

Now you can run `molecule`:

```shell
molecule test --destroy=never
molecule login
```
