# Git Repository


- a local repository resides on your computer
-



## Default branch

Git creates a default branch called `main`


```shell
git config --global init.defaultBranch main
```


```config title="~/.config/git/config"
[init]
	# default branch name when creating a new local repository
	defaultBranch = main
```


## Remote Branches

Use the `--track` flag with git checkout for remote branches that are not yet local.

!!! NOTE "Checkout a remote branch, creating a local branch of the same name"
    ```shell
    git checkout --track origin/remote-branch-name
    ```


??? EXAMPLE "Output of Checkout Remote Branch"
    ```shell-output
    $ git checkout --track origin/remote-branch-name

    Branch remote-branch-name set up to track remote branch newsletter from origin.
    Switched to a new branch 'remote-branch-name'
    ```

A local branch is created using the unqualified name of the remote branch, `remote-branch-name`. This local branch is used to track the remote branch, allowing changes to be pulled and pushed.
