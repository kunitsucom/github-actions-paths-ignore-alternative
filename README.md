# GitHub Actions `paths-ignore` Alternative

A workaround workflow to resolve the incompatibility of the `Require status checks to pass` setting when `paths-ignore` is set for push trigger or pull_request trigger in GitHub Actions.

```yml
name: example

on:
  push:
    # NO paths-ignore
  pull_request:
    # NO paths-ignore
  workflow_dispatch:

jobs:
  paths-ignore:
    runs-on: ubuntu-latest
    outputs:
      skip: ${{ steps.paths-ignore.outputs.skip }}
    steps:
      - uses: kunitsucom/github-actions-paths-ignore-alternative@main
        id: paths-ignore
        with:
          paths-ignore: |-
            # substrings of file paths to ignore written in regular expressions
            ^.github/
            ^.*\.md$
            ^.*\.log$
  # Note: A job that is skipped will report its status as "Success". It will not prevent a pull request from merging, even if it is a required check.
  # ref. https://docs.github.com/en/actions/using-jobs/using-conditions-to-control-job-execution#overview
  not-skip:
    runs-on: ubuntu-latest
    needs: paths-ignore
    if: ${{ needs.paths-ignore.outputs.skip != 'true' }}
    steps:
      - name: "if not skip"
        run: |
            echo "needs.paths-ignore.outputs.skip: ${{ needs.paths-ignore.outputs.skip }}"
  skip:
    runs-on: ubuntu-latest
    needs: paths-ignore
    if: ${{ needs.paths-ignore.outputs.skip == 'true' }}
    steps:
      - name: "if skip"
        run: |
            echo "needs.paths-ignore.outputs.skip: ${{ needs.paths-ignore.outputs.skip }}"
```
