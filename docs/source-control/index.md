# Source Control

Managing code changes is an important part of maintaining a usable code base and is an essential step to having a robust development workflow.

Versioning code tracks all the changes over the life of a project, providing an audit trail of change.

## Git

Git is the defacto tool to manage code and other text artifacts of a project.

Git uses a distributed model for sharing changes which also encourages branching to support easily experimenting with changes.


## GitHub and GitLab

Code sharing services support Git and also add pull requests to enable people to contribute to projects they are not maintainers for.  This is especially useful for open source projects, allowing the community to contribute.


!!! INFO "Emoji guide for commit messages and pull requests"
    [gitmoji](https://gitmoji.dev/) provides an interactive search for emoji that can be used with Git commit messages and pull request descriptions & templates

    [emojipedia](https://emojipedia.org/github) provides a wider support for emoji across multiple services



!!! HINT "Collective Ownership of Code"
    The distributed nature of Git supports and encourages collective ownership of the project code, as all developers take responsibility for the project assets



## Access remote repositories

Sharted Git repositories can be accessed via HTTPS or SSH URL.

SSH approach is typically more secure, especially as the files holding your keys on disk are encrypted.  SSH connections can be tunnelled through HTTPS if connecting to a remote repository via a very restricted firewall.

HTTPS connection requires a personal access token is required (GitHub blocks HTTP access via password).  A personal access token can be create with limited access, only allowing access to specific services and information. 

A personal access token is saved in a plain text file, e.g. `~/.github`.  Should a token be compromised, it does not give access to the account on the remote repository, so the token can be deleted and recreated without compromising the service login account.


## What to Version

Text based files such as source code, unit and acceptance tests, web pages, cascading stlye sheets, database scripts and configuration files should all be versioned to ensure that at any time you can build the whole project from the version control system.

Binary files such as images and logos should also be versioned, even though you cannot usually do meaningful diffs.  It is important to version those files that are part of the software project so your build, test and release scripts are always using the correct version.

It is important to also version your build files (build.xml, myproject.pom) so that you always have the correct script versions to build, test and deploy your project.  If all your assets are in the version control system, it make it very easy to set up and maintain a continuous integration server.

Compiled assets such as JavaDoc output, class files, jar and war files are not usually stored in the version control system as these can be generated.  This avoids duplication of information and ensures that information does not become outdated.


