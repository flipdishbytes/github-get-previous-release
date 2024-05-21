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
  contents: read # grants permissions to read gh releases

jobs:
  get-previous-version:
    runs-on: ubuntu-latest
    outputs:
      previousReleaseTag: ${{ steps.previous_version.outputs.previousReleaseTag }}
    steps:
      - name: Get previous version
        id: previous_version
        uses: flipdishbytes/github-get-previous-release@v1.0
        with:
          repositoryName: ${{ github.repository }}

  deploy-previous-version:
    runs-on: ubuntu-latest
    needs:
      - get-previous-version

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          ref: ${{ needs.get-previous-version.outputs.previousReleaseTag }} # checkout the previous release tag
      
      - name: Echo the previousReleaseTag
        run: echo $VERSION
        env:
          VERSION: ${{ needs.get-previous-version.outputs.previousReleaseTag }}
```