# GitHub Workflows and Actions

![GitHub Actions Logo](https://github.com/practicalli/graphic-design/blob/live/icons/github-actions-trans.png?raw=true){align=right loading=lazy}

A [GitHub workflow](./workflow/index.md) defines one or more jobs to carry out as part of a continuous integration run.

YAML is used to define a workflow.

Workflows are [triggered by events](./trigger-events.md), most commonly by a Git related event (commit, Pull Request, etc.).

Workflows can be schedule to run at a specific time and day, using the common cron syntax.

Including `workflow_dispatch:` as a trigger, a human can manually trigger a workflow by viewing it in the Actions section of the GitHub website.

??? INFO "GitHub Website menu uses Actions rather than Workflows 🤷"

[GitHub Actions](./actions.md) provide common jobs that can be used within a workflow, greatly reducing the configuration required to get tasks done.

A workflow is a list of `jobs:` that defines what the workflow does, each job is defined with a `name`. Practicalli has several [common jobs](./common-jobs.md) included in most workflows.

[Practicalli Workflows](./workflow/practicalli.md){.md-button}
