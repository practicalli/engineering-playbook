# Git Ignore patterns

Define a list of patterns to ensure unwanted files and directories are not committed to the local Git repository.  This is especially important for files that contain sensitive information, e.g. access codes, etc.

A common concern with defining  approach is that OS and tool specific files get included, especially as more software engineering teams work with the project.  This leads to messy commits that include changes to the Git ignore file or extra commits specifically for Git ignore patterns.

- use [a global ignore file](#global-ignore-patterns) for OS, tools and temporary files
- use [an inclusive ignore file](#including-patterns) to define the patterns that define files to include

!!! TIP "Use Global Ignore for tool specific files"
    Avoid polluting project `.gitignore` file with tooling specific patterns which should be added to [a global ignore file](#global-ignore-patterns) that is applied to all projects


## Excluding Patterns

The classic approach is to define patterns that exclude files and directories that should not be committed to the local Git repository.

- `*` represents any number of characters within a file or directory name.
- `**/` followed by a pattern will try match that to any level of the file system hierarchy from the root of the project
- `[Tt]`

!!! NOTE "Exclude all files except common project files"
    ```config title="~/config/git/gitignore"
    ## ------ General Editor Tooling ------ ##
    .idea
    .iml
    *.sublime-workspace
    .vscode

    ## ------ Developer Tooling --------- ##
    **/.calva
    **/.carve
    **/.cpcache/
    **/.clj-kondo/.cache
    **/.clj-kondo/.cache/
    **/.lsp
    **.lsp/.cache/
    **/.clojure-lsp
    .rebel_readline_history

    ## ------ Lint & Format tools ------ ##
    **/megalinter-reports/

    ## ------ Logs and databases ------ ##
    *.log
    **/sqlite.db
    **/dump*

    ## ------ OS generated files ------ ##
    desktop.ini
    *.DS_Store
    .DS_Store?
    ._*
    .Spotlight-V100
    .Trashes
    [Tt]humbs.db

    ## ------ Secure files ----- ##
    **/*.gpg

    ## ------ Temporary files ----- ##
    *~
    *#
    \#*
    \#
    .\#*
    ._*
    auto-save-list
    *.bak
    /.emacs.desktop
    /.emacs.desktop.lock
    .elc
    *.swo
    *.swp
    tramp
    ```

## Including Patterns

Avoid maintaining a long list of files to exclude by configuring an ignore file to exclude all files by default.

Then add patterns prepended with `!` to define files and directories to include.

This approach greatly minimises changes or accidental file commits, especially when using a standard project structure.

!!! NOTE "Exclude all files form the root of the project"
    ```config title="~/config/git/gitignore"
    # Ignore everthing in root directory
    /*
    ```

!!! NOTE "Exclude all files except common project files"
    ```config title="~/config/git/gitignore"
    # Ignore everthing in root directory
    /*

    # Common project files
    !CHANGELOG.md
    !README.md
    !LICENSE
    ```

??? EXAMPLE "Git inclusive patterns for Clojure projects"
    ```config
    # ------------------------
    # Clojure Project Git Ignore file patterns
    #
    # Ignore all except patterns starting with !
    # Add comments on separate lines, not same line as pattern
    # ------------------------

    # ------------------------
    # Ignore everthing in root directory
    /*

    # ------------------------
    # Common project files
    !CHANGELOG.md
    !README.md
    !LICENSE

    # ------------------------
    # Include Clojure project & config
    !build.clj
    !deps.edn
    !pom.xml
    !dev/
    !docs/
    !resources/
    !src/
    !test/

    # ------------------------
    # Include Clojure tools
    !.cljstyle
    !.dir-locals.el
    !compose.yaml
    !Dockerfile
    !.dockerignore
    !**/.clj-kondo/config.edn
    !Makefile
    !tests.edn

    # ------------------------
    # Include Git & CI workflow
    !.gitattributes
    !.gitignore
    !.github/
    ```


## Global Ignore patterns

Operating System and development tool specific meta files are common across all projects for a specific software engineer.

Defining a file with global ignore patterns with these common files ensures they are excluded across all projects.

A global ignore file reduces the changes required to a project specific `.gitignore` file, helping to keep a cleaner commit history.

Create a `~/config/git/gitignore-global` file  with common patterns to ignore across all projects.


??? EXAMPLE "Common OS and tooling file patterns to ignore"
    ```config title="~/config/git/gitignore-global"
    # An example global gitignore file
    #
    # If using XDG basedir configiration, save this file to the root of the users git configuration directory
    # e.g. `$XDG_CONFIG_HOME/git/ignore-global`
    # Run `git config --global core.excludesfile ignore-global`

    # Otherwise add this file to $HOME/.gitignore-global
    # Run `git config --global core.excludesfile $HOME/.gitignore-global`

    ## ------ Clojure Tooling ------ ##
    **/.calva
    **/.carve
    **/.cpcache/
    **/.clj-kondo/.cache
    **/.clj-kondo/.cache/
    **/.lsp
    **.lsp/.cache/
    **/.clojure-lsp
    .rebel_readline_history

    ## ------ General Editor Tooling ------ ##
    .idea
    .iml
    *.sublime-workspace
    .vscode

    ## ------ Lint & Format tools ------ ##
    **/megalinter-reports/

    ## ------ Logs and databases ------ ##
    *.log
    **/sqlite.db
    **/dump*

    ## ------ OS generated files ------ ##
    desktop.ini
    *.DS_Store
    .DS_Store?
    ._*
    .Spotlight-V100
    .Trashes
    [Tt]humbs.db

    ## ------ Archives ------ ##
    # it's better to unpack these files and commit the raw source
    # git has its own built in compression methods
    *.7z
    *.dmg
    *.gz
    *.iso
    *.jar
    *.msi
    *.rar
    *.tar
    *.war
    *.xz
    *.zip

    ## ------ Secure files ----- ##
    **/*.gpg

    ## ------ Temporary files ----- ##
    *~
    *#
    \#*
    \#
    .\#*
    ._*
    auto-save-list
    *.bak
    /.emacs.desktop
    /.emacs.desktop.lock
    .elc
    *.swo
    *.swp
    tramp
    ```

Add the global ignore patterns to your Git configuration

!!! NOTE "Set Global Excludes pattern file"
    ```shell
    git config --global core.excludesfile ~/config/git/gitignore-global
    ```
