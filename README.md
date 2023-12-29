# Pull Request Target Branch Check

This GitHub Action checks the target branch of a pull request and helps to avoid merging it to a wrong one.

This is done using a branch file (`.branch` is the default file). The content of the file is the name of its branch. When a pull request is raised, the action checks for the branch file in the source branch. If it's present, the action checks whether the content of the file matches the target branch. If it matches the check is passed. If not the check results in error. If the file is not present in the source repository this check is ignored.

##  Inputs

| Name          | Required | Default Value                 | Description                                                                    |
|---------------|----------|-------------------------------|--------------------------------------------------------------------------------|
| branch_file   | false    | .branch                       | Name of the file. The content of this file should match the name of its branch |
| source_branch | false    | origin/${{ github.head_ref }} | The source branch of the pull request                                          |

## Usage

```yaml
---
on:
  pull_request:
    branches:
      - develop

permissions: {}

jobs:
  checks:
    runs-on: ubuntu-latest
    timeout-minutes: 5
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4
        with:
          fetch-depth: 2

      - name: Pull Request Target Branch Check
        uses: drankhil/pull-request-target-branch-check@f94f50106cda2928f903d79e723a7495af33af54  # v1.0.0
        with:
          source_branch: ${{ github.event.pull_request.head.sha }}  # This is not required if fetch-depth is set to 0
```

## Output examples

When branch_file exist and the contents matches

```
INFO: The content of file ".branch" matches the target branch "release/stable-2.x"
```

When branch_file exist and the contents doesn't match

```
ERROR: Wrong target branch. The content of ".branch" doesn't match the target branch "main"
```

When the branch_file doesn't exist

```
INFO: Skipping the check. Branch file ".branch" doesn't exist on "release/stable-1.x"
```
