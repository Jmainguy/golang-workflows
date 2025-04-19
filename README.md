# golang-workflows

This repository is for managing github workflows for the purpose of automating the Continuous Integration of a Golang codebase.


## Usage

Place a two files in your repos .github/workflows/ directory

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
    uses: Jmainguy/golang-workflows/.github/workflows/golang-release.yml@v1
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}
      docker_username: ${{ secrets.DOCKER_USERNAME }}
      docker_password: ${{ secrets.DOCKER_PASSWORD }}
    with:
      docker_url: zot.soh.re
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
    uses: Jmainguy/golang-workflows/.github/workflows/golang-ci.yml@v1
    with:
      docker: true
```
