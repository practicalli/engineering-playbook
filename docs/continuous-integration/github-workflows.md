# GitHub workflows

GitHub workflows used by Practicalli

## Changelog Update Check

Check the CHANGELOG.md file has been updated for a pull request, providing a reminder to add a summary of changes for the pull request

Defines `changelog-check-skip` label on a pull request instructs the workflow not to run

```yaml title=".github/workflows/changelog-check.yml"
---
name: Changelog Check
on:
  pull_request:
  paths-ignore:
    - "README.md"
    - "CHANGELOG.md"
  types: [opened, synchronize, reopened, ready_for_review, labeled, unlabeled]

jobs:
  # Check CHANGELOG.md file updated on every pull request
  changelog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: dangoslen/changelog-enforcer@v2
    with:
      changeLogPath: "CHANGELOG.md"
      skipLabels: "changelog-check-skip"
```


## mkdocs publisher

```yaml
---
name: Publish Book
on:
  # Manually trigger workflow
  workflow_dispatch:

  # Run work flow conditional on linter workflow success
  workflow_run:
    workflows:
      - "MegaLinter"
    paths-ignore:
      - README.md
      - CHANGELOG.md
      - .gitignore
    branches:
      - main
    types:
      - completed

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - uses: actions/setup-python@v4
        with:
          python-version: 3.x
      - uses: actions/cache@v3
        with:
          key: ${{ github.ref }}
          path: .cache
      - run: pip install mkdocs-material mkdocs-callouts mkdocs-glightbox mkdocs-git-revision-date-localized-plugin pillow cairosvg
      - run: mkdocs gh-deploy --force
```
