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
