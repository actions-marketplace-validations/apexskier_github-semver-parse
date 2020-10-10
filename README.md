# Semver Parse

This GitHub Action parses semver strings.

## Example

```yml
name: Tag creation

on:
  push:
    tags:
      - v*

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Get the version
        id: version
        run: echo ::set-output name=VERSION::${GITHUB_REF/refs\/tags\//}
        shell: bash

      - uses: apexskier/github-semver-parse@v1
        id: semver
        with:
          version: ${{ steps.version.outputs.VERSION }}

      - name: Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.RELEASE_GH_TOKEN }}
        with:
          tag_name: ${{ steps.version.outputs.VERSION }}
          release_name: ${{ steps.version.outputs.VERSION }}
          prerelease: ${{ !!steps.semver.outputs.prerelease }}
```
