# Git Status

Show the status of the files in the source controlled project, comparing the working copy with the latest commit on the current branch.

A file can have one of the following status values

- untracked
- modified
- staged

> A file can have both modified and staged statuses if only specific lines or hunks were staged.

## Git Alias

Define a command line alias for Git, e.g. `git sr` or `git sitrep` that includes status command flags to show a short-format report that also includes the branch name

!!! EXAMPLE "Define Git alias for status"
    ```config title=".config/git/config"
    [alias]
     sitrep = status --short --branch
     sr = status --short --branch
    ```

## Report across repositories

Use the `mgitstatus` command line tool to show the status of all the repositories under a give directory

```shell
cd ~/projects/practicalli && mgitstatus
```

The status includes the following types:

- ok - no changes
- untracked files
- uncommitted changes
- needs push (branchs)
- needs pull (branches)

![mgitstatus example](https://github.com/practicalli/graphic-design/blob/live/git/git-status-multiple-repositories-mgitstatus.png?raw=true)
