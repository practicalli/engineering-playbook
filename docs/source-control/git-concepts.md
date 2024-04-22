# Git Concepts

## Repository
A repository is the magic box that contains all the information about your checked in items, the full history of your changes.

## Trunk
The trunk refers to the main set of files and changes that make up the project.    If you create a new repository and commit / push a change (eg. a new file) then you have added that change to the trunk.

A trunk is what you typically release and refer to as the main version of the project.

## Branch
A project under version control may be branched to work on new functionality or experimental development.  The branch becomes a separate independent copy, even though it is not actually be stored as a copy in the version control system.

Changes committed / pushed to the trunk after the branch are not applied to the branch and changes committed / pushed to the branch are not applied to the trunk.

## Tag
A tag or label refers to an important snapshot in time of your repository. The current version of all files can be tagged with a meaningful name or revision number. Tags are often used to indicate milestones or releases of a project.

## Head
The head is a special label that represents the latest version of your files in the repository.  When you checkout or clone a project you get the version that is pointed to by the head label - unless you specify a particular version.

## Commit
A commit (check-in) is the action of writing or merging the changes made in the working copy back to the repository. The terms 'commit' and 'check-in' can also used in noun form to describe the new revision that is created as a result of committing.

## Update
An update merges changes made in the central repository (by other people, for example) into the local working copy / local repository

## Clone
Creates a complete copy of a repository locally (used in distributed version control systems such as Git, Mercurial, Bazaar)


## Push
Merges change sets from your local repository to the main (parent) of your repository.

## Pull
Merge change sets from other repositories - you pull in change sets and merge them into your repository

## Promote
A more abstract term for committing or pushing, where changes are "promoted" from a less controlled location into a more controlled location. For example, from a user's workspace into a repository, or from a local clone to its main (parent).


## Diff
A diff (or delta) represents a specific change to a project under version control.  Diffs between different versions of files are created to see what changes have been between two versions and giving you information on how to merge two files together.


## Merge
A merge where two sets of changes are applied to a file or set of files. Some sample scenarios for merging include:

You update your working copy or local repository with changes made by other users, either from a central repository or from another repository.

You try to commit / push changes to files that have been updated by others since you checked out those files (or you last update).  The version control software automatically merges the files (typically, after prompting the user if it should proceed with the automatic merge, and in some cases only doing so if the merge can be clearly and reasonably resolved).

The project is branched before a problem that existed is fixed in the main trunk.  The fix is done in the trunk and then merged into the branch.

A branch is created, the code in the files is independently edited, and the updated branch is later incorporated into a single, unified trunk.


## Conflict
A conflict occurs when different changes are being applied to the same file, and the version control system is unable to reconcile the changes. The person making a merge needs to resolve the conflict by combining the changes manually, or by telling the version control system which version to use.


# Diff and Merge

When you want to merge code you may experience conflicts between your code and the code your merging.  If your version control tool is not able to automatically manage these conflicts, you will need to merge manually.

To see the differences between two code files you use a diff tool.  A diff tool will show text references or a visual representation of the differences.

Using the diff tool you can change your code so the conflicts are addressed and changes can be merged.


!!! HINT "Meld - visual diff and merge tool"
    Meld is a visual diff and merge tool. You can compare two or three files and edit them in place (diffs update dynamically). You can compare two or three folders and launch file comparisons. You can browse and view a working copy from popular version control systems such such as CVS, Subversion, Bazaar-ng and Mercurial.

    Calling Meld from git-merge

    ```shell
    git mergetool -t meld
    ```


## Git Advanced branching and merging

All of the changes that git was able to merge automatically are already added to the index file, so git diff shows only the conflicts. It uses an unusual syntax:

The commit which will be committed after we resolve this conflict will have two parents instead of the usual one: one parent will be HEAD, the tip of the current branch; the other will be the tip of the other branch, which is stored temporarily in MERGE_HEAD.

http://book.git-scm.com/5_advanced_branching_and_merging.html

