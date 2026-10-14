# Source Control

Managing code changes is an important part of maintaining a usable code base and an essential step to having a robust development workflow.

Versioning code tracks all the changes over the life of a project, providing an audit trail of change.

## Git

Git is the defacto tool to manage code and other artifacts of a project.

A Git project is created locally for a new project, in the `.git` directory.  Changes are included in the local repository, first by adding to a staged state using `git add`, then committing to the local repository using `git commit`

Git uses a distributed model for sharing changes and also encourages branching to support easily experimentation.

!!! HINT "Collective Ownership of Code"
    The distributed nature of Git supports and encourages collective ownership of the project code, as all developers take responsibility for the project assets

??? "Conventional Commits - a structure for commit messages"
    [Conventional commits specification](https://www.conventionalcommits.org/) is an easy set of rules for describing the features, fixes, and breaking changes made in commit messages.  This convention creates an explicit commit history which is simple to process with automated tools, e.g. highlighting breaking changes.

??? INFO "Emoji guide for commit messages and pull requests"
    [gitmoji](https://gitmoji.dev/) provides an interactive search for emoji that can be used with Git commit messages and pull request descriptions & templates

    [emojipedia](https://emojipedia.org/github) provides a wider support for emoji across multiple services


## Code Sharing Services

Git is used to efficiently share changes to a project via a repository on the shared service, with either public or private access.

Contributions are made by pushing commits directly to the remote repository by maintainers of the project, using `git push`.  Shared contributions to the repository can be obtained by anyone who has access using `git pull`.

Pull Requests (PRs) include a review step to contributions, optionally with continuous integration workflows.  Pull Requests also allow people other than the project maintainers to contribute to a repository, especially useful for open source projects.


[GitHub](https://github.com/){target=_blank .md-button} [GitLab](https://gitlab.com/){target=_blank .md-button} [BitBucket](https://bitbucket.com/){target=_blank .md-button}


## Access remote repositories

Sharted Git repositories can be accessed via HTTPS or SSH URL.

SSH uses [Public-key_cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) where a private key securely encrypts information and a public key validates the identity of a commit.

HTTPS connection requires a personal access token is required (GitHub blocks HTTP access via password).  A personal access token can be create with limited access, only allowing access to specific services and information.

A personal access token is saved in a plain text file, e.g. `~/.github`.  Should a token be compromised, it does not give access to the account on the remote repository, so the token can be deleted and recreated without compromising the service login account.

## What to Version

Text based files such as source code, unit and acceptance tests, web pages, cascading stlye sheets, database scripts and configuration files should all be versioned to ensure that at any time you can build the whole project from the version control system.

Binary files such as images and logos should also be versioned, even though you cannot usually do meaningful diffs.  It is important to version those files that are part of the software project so your build, test and release scripts are always using the correct version.

It is important to also version your build files (build.xml, myproject.pom) so that you always have the correct script versions to build, test and deploy your project.  If all your assets are in the version control system, it make it very easy to set up and maintain a continuous integration server.

Compiled assets such as JavaDoc output, class files, jar and war files are not usually stored in the version control system as these can be generated.  This avoids duplication of information and ensures that information does not become outdated.
