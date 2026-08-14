# MkDocs Static Site Generator

MkDocs is a python tool for generating documentation sites from Markdown.

Material for MkDocs is a theme providing a rich autoring experience to create engaging websites.


!!! WARNING "MkDocs version 2.x is an incompatible re-write"
    Material for MkDocs will not work with MkDocs 2.x due to significant breaking changes in the MkDocs API from version 1.x


!!! INFO "Practicalli is migrating to Zensical through 2026"
    [Zensical Static Site Generator](zensical.md){.md-button} is a complete rewrite of MkDocs and Material for MkDocs.

    Zensical is designed for high performance and addresses a large number of limitations with the original MkDocs and Material for MkDocs projects.

    Zensical is compatible with existing Material for MkDocs projects, although not all plugins are supported until latter 2026 (e.g. blog support not officially available yet).


## Install

Practicalli recommends installing Material for MkDocs as a tool, using the Uv package manager.

This approach ensures isolation of Python packages whist exposing `mkdocs` as a CLI command.


=== "Uv tool install"

    A bash script to install MkDocs with Material for MkDocs and other plugins relevant to Practicalli websites.

    Uv will install the latest MkDocs version, up to a maximum version of `1.9.9`.

    The `--update` flag is required to install a newer version when an earlier version is already installed.

    ```shell
    #!/usr/bin/env bash

    # -----------------------------------------------
    # Python packages as tools (uv)
    # run as user (not root)

    if [ "$(whoami)" = "root" ]; then
     echo "Run the uv tool install script with your user account. Do not run as root or via sudo"
     exit
    fi

    echo "# ---------------------------------------"
    echo "Install Material for MkDocs as tool with supporting plugins"
    uv tool install mkdocs'<=1.9.9' --with mkdocs-material --with mkdocs-callouts --with mkdocs-glightbox --with mkdocs-git-revision-date-localized-plugin --with mkdocs-redirects --with mkdocs-rss-plugin --with pillow --with cairosvg --upgrade
    echo "# ---------------------------------------"
    echo
    ```
