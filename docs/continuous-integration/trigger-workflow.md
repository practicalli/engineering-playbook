# Trigger Workflows

[GitHub workflows are triggered by events](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows).

Events related to a GitHub workflow include

- pull request
- push
- workflow_dispatch (Manual run)
- [Schedule (Cron)](#cron-schedule)


!!! EXAMPLE "Scheduled Version Check"
    [Scheduled Version Check](/continuous-integration/github-workflow/#scheduled-version-check) GitHub workflow uses the cron schedule to check for newer versions of Clojure libraries and GitHub actions


## Cron schedule

GitHub workflows can use `cron:` to define a schedule as to when a workflow should run.

A POSIX Cron pattern is used to define the schedule

- Minute [0,59]
- Hour [0,23]
- Day of the month [1,31]
- Month of the year [1,12]
- Day of the week ([0,6] with 0=Sunday)

```yaml
    name: "Scheduled Version Check"
    on:
      schedule:
        - cron: "0 4 * * *" # at 04:04:04 ever day
        # - cron: "0 4 * * 5" # at 04:04:04 ever Friday
        # - cron: "0 4 1 * *" # at 04:04:04 on first day of month
```

!!! INFO "Scheduled version check uses cron schedule"


??? INFO "Cron references"
    - [`on.schedule` - GitHub workflow syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onschedule)
    - [Cron Expression Syntax Cheatsheet](https://cronstatus.com/docs/cron/)



## Workflow Dispatch

Add `workflow_dispatch:` without arguments to the `on:` directive in a workflow configuration file to manually trigger the running of the workflow.

Visit GitHub.com > Actions > Workflow Name in the repository and select `Run`


## Conditional on other workeflow

Run a workflow based on the outcome of running another GitHub workflow

!!! EXAMPLE "Run workflow if MegaLinter workflow returns success"
    ```yaml
        name: "Publish Documentation"
        on:
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
    ```
