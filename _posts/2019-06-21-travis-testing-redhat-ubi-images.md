---
title: Ansible Molecule tests using Red Hat UBI images
---

# Ansible Molecule tests using Red Hat UBI images

[Red Hat](https://www.redhat.com/en) now offers [Universal Base Images(UBI)](https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image). These images are stored on [Red Hat's registry](https://access.redhat.com/containers/):

- [UBI Red Hat Enterprise Linux 7](https://access.redhat.com/containers/?tab=overview#/registry.access.redhat.com/ubi7/ubi)
- [UBI Red Hat Enterprise Linux 8](https://access.redhat.com/containers/?tab=overview#/registry.access.redhat.com/ubi8/ubi)

That's great, now everybody can test on a Red Hat container.

There are a few hoops you have to jump through to be able to use it in Travis:

## Store credentials in Travis

The Red Hat Registry [documents that a username and password need to be set](https://access.redhat.com/containers/?tab=images&get-method=registry-tokens#/registry.access.redhat.com/ubi8/ubi) to be able to pull:

```
$ docker login registry.redhat.io
Username: ${REGISTRY-SERVICE-ACCOUNT-USERNAME}
Password: ${REGISTRY-SERVICE-ACCOUNT-PASSWORD}
Login Succeeded!

$ docker pull registry.redhat.io/ubi7/ubi
```

You need to generate a [service-account](https://access.redhat.com/terms-based-registry/) once.

Paste the username and password in Travis, under the build -> more options -> settings - environment variables.

Be sure to escape weird characters. My username contains a `|`, for example: `foo|bar` needs to be entered as `foo\|bar`.

## Change molecule.yml

[Molecule](https://molecule.readthedocs.io/) can pickup environment variables as [documented](https://molecule.readthedocs.io/en/stable/configuration.html#variable-substitution).

So, your `molecule.yml` may end up something like this:

(Reduced example, your milage may vary)

```yaml
---
dependency:
  name: galaxy
  options:
    role-file: requirements.yml
driver:
  name: docker
platforms:
  - name: bootstrap-rhel-latest
    image: ubi8/ubi
    registry:
      url: registry.access.redhat.com
      credentials:
        username: $registryredhatiousername
        password: $registryredhatiopassword
scenario:
  name: default
```
