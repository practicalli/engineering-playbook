# Commits

Changes are managed through a series of commits added to a repository.

Types of commit:

- commit: create a new commit
- amend: update the current head commit, optionally modifying the commit message
- extend: update the current head commit without editing the message

!!! WARNING "amend and extend rewrite history"
    Avoid changing commits already shared to a remote repository, as it will cause merge conflicts for people actively using the project.


## Conventional commits

Structure commit messages to make them easy to parse for people and tools.

[:fontawesome-solid-book-open: Conventional Commits overview](../conventional-commits.md){target=_blank .md-button}
