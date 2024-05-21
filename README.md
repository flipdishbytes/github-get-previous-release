# Purpose

This custom GitHub Action were created to integrate GitHub Actions releases rollback. It allows you to get previous Release in GitHub. If version is provided it outputs the same version, else it gets the previous release tag ans set outpit variable called previousReleaseTag

# Github Action: Get Previous Release `flipdishbytes/github-get-previous-release@v1.0`

To use this action, add it to your pipeline workflow YAML file. Here are examples.

### How it works?

1. Reads `Version` variable.
2. If no `Version` is provided it reads last 15 GitHub releases as a limit using `gh release list` and gets the `isLatest` release. `main.2024.05.08.2.db14289` as example.
2. Returns result as `previousReleaseTag` output.

### How to use?

#### `flipdishbytes/github-get-previous-release@v1.0`

```yaml
name: GH Action workflow with Creating GH Release and Sending Slack Notification

on:
  push:
    branches:
      - main

permissions:
  contents: read # grants permissions to create gh releases

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Get previous version
        uses: flipdishbytes/github-get-previous-release@v1.0
        with:
          repositoryName: ${{ github.repository }}

```