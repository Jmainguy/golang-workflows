# golang-workflows

This repository is for managing github workflows for the purpose of automating the Continuous Integration of a Golang codebase.


## Usage

Place two files in your repository's `.github/workflows/` directory. When the
release includes a container, configure GoReleaser/ko to publish to
`$KO_DOCKER_REPO`; the workflow then resolves and signs its immutable digest.

release.yaml
```yaml
name: Golang Release

on:
  push:
    branches:
      - main

permissions:
  contents: write
  pull-requests: write

jobs:
  release-please:
    uses: Jmainguy/golang-workflows/.github/workflows/golang-release.yml@v2
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}
      docker_username: ${{ secrets.DOCKER_USERNAME }}
      docker_password: ${{ secrets.DOCKER_PASSWORD }}
      notation_private_key: ${{ secrets.NOTATION_PRIVATE_KEY }}
      notation_certificate_chain: ${{ secrets.NOTATION_CERTIFICATE_CHAIN }}
      notation_ca_certificate: ${{ secrets.NOTATION_CA_CERTIFICATE }}
    with:
      docker_url: zot.soh.re
      registry_namespace: jmainguy
      image_name: ${{ github.event.repository.name }}
```

ci.yml
```yaml
name: Golang CI

on:
  push:

permissions:
  actions: read
  contents: read
  security-events: write

jobs:
  golang-ci:
    uses: Jmainguy/golang-workflows/.github/workflows/golang-ci.yml@v2
    with:
      docker: true
```
