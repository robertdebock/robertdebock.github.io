---
title: GitHub action to release to Galaxy
---

# GitHub action to release to Galaxy

[GitHub Actions](https://github.com/features/actions) is an approach to offering CI, using other peoples actions from the [GitHub Action Marketplace](https://github.com/marketplace?type=actions).

I'm using 2 actions to:
1. test the role using Molecule
2. release the role to Galaxy

GitHub is offering [20 concurrent builds](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/about-github-actions#usage-limits) which is quite a lot, more than [Travis](https://travis-ci.org/)'s limit of 5. The build could be 4 times faster. Faster CI, happier developers. ;-)

Here are a few examples. First; just release to Galaxy, no testing includes. (Not a smart idea)

```yaml
---
name: GitHub Action

on:
  - push

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v2
      - name: galaxy
        uses: robertdebock/galaxy-action@master
        with:
          galaxy_api_key: ${{ secrets.galaxy_api_key }}
```

As you can see, 2 actions are used, `checkout` which gets the code and `galaxy-action` to push the role to [Galaxy](https://galaxy.ansible.com/). Galaxy does lint-testing, but not functional testing. You can use the `molecule-action` to do that.

```yaml
---
name: GitHub Action

on:
  - push

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v2
        with:
          path: "${{ github.repository }}"
      - name: molecule
        uses: robertdebock/molecule-action@1.0.0
        with:
          image: ${{ matrix.image }}
  release:
    needs:
      - test
    runs-on: ubuntu-latest
    steps:
      - name: galaxy
        uses: robertdebock/galaxy-action@master
        with:
          galaxy_api_key: ${{ secrets.galaxy_api_key }}
```

The build is split in 2 parts now; `test` and `release` and `release` needs `test` to be done.
You can also see that `checkout` is now called with a `path` which allows Molecule to find itself. (`ANSIBLE_ROLES_PATH: $ephemeral_directory/roles/:$project_directory/../`)

Finally you can include a `matrix` to build with a matrix of variables set.

```yaml
---
name: GitHub Action

on:
  - push

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        image:
          - alpine
          - amazonlinux
          - debian
          - centos
          - fedora
          - opensuse
          - ubuntu
    steps:
      - name: checkout
        uses: actions/checkout@v2
        with:
          path: "${{ github.repository }}"
      - name: molecule
        uses: robertdebock/molecule-action@1.0.0
        with:
          image: ${{ matrix.image }}
  release:
    needs:
      - test
    runs-on: ubuntu-latest
    steps:
      - name: galaxy
        uses: robertdebock/galaxy-action@master
        with:
          galaxy_api_key: ${{ secrets.galaxy_api_key }}
```

Now your role is tested on the list of `images` specified.

Hope these actions make it easier to develop, test and release your roles, if you find problems, please make an issue for either the [molecule](https://github.com/robertdebock/molecule-action/issues) or [galaxy](https://github.com/robertdebock/galaxy-action/issues) action.
