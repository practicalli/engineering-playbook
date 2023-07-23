# GitHub workflows

The marketplace page for a GitHub action should specify how it is used within a GitHub workflow.

## Event Triggers

Workflows can be triggered by 

- commit to branch
- `pull_request`
- `cron` scheduled event
- `workflow_dispatch` manual trigger

## Include Exclude patterns

Files and directories to include or exclude within the scope of a trigger.  

e.g. ingnore a pull request when changes are only in the `README.md` file

```yaml
    name: Workflow name
    on:
      pull_request:
        paths-ignore:
          - "README.md"
```


## GitHub Action version

GitHub actions typically use semantic versioning for their releases

??? INFO "Semantic versioning"
    Given a version number MAJOR.MINOR.PATCH, increment the:

    - MAJOR version for incompatible API changes
    - MINOR version to add functionality in a backward compatible manner
    - PATCH version for backward compatible bug fixes
    - Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

    [Semantic Versioning 2.0.0 Specification](https://semver.org/){target=_blank .md-button} 


Specify the major release the major version will use the latest version within that scope, e.g. `action/checkout@v3` will use `v3.5.2`, the latest version within that major version

```yaml
steps:
    - uses: actions/checkout@v3
```

??? HINT "Use major version of Action in workflow configuration"
    Use the major version of a GitHub action within a GitHub workflows to minimise maintenance of the workflow configuration.


Use a specific patch release tag when a specific version is reqiured, providing a consistent version that is used each time.

```yaml
steps:
    - uses: actions/checkout@v1.0.1
```

Use a branch name, commonly used for release management

```yaml
steps:
    - uses: actions/checkout@v1-beta
```

Using a commit's SHA for release management
Every Git commit has a unique and immutable SHA value, calculated in part from the contents of the commit. 

A SHA value can be more reliable than specifying a tag value which could be deleted or moved.

Using a SHA value does means only that specific commit is used, so a new SHA value must be used if a newer version is required. The full SHA value of the commit must be used, not the abbreviated value.

```yaml
steps:
    - uses: actions/checkout@172239021f7ba04fe7327647b213799853a9eb89
```

[:fontawesome-brands-github: GitHub: creating actions reference](https://docs.github.com/en/actions/creating-actions/about-custom-actions){target=_blank .md-button}


