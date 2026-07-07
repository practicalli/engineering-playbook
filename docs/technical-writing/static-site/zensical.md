# Zensical Static Site Generator

![Zensical Logo](https://zensical.org/assets/zensical.svg){align=right loading=lazy style="height:150px;width:150px"}

!!! QUOTE ""
    A scalable Open Source systems for technical writing that always keep you in the flow

[:globe_with_meridians: Zensical](https://github.com/zensical/zensical){target=_blank} is a Rust & Python tools for generating documentation sites from Markdown (potentially Common Mark in future).

Zensical is a very efficient tool that generates a site from Markdown within a few seconds (an order of magnitude faster than most generator tools).

Zensical is a rapidly evolving tool that aims to be feature comparable with Material for MkDocs by the end of 2026.


`zensical.toml` is used to configure a project, defining plugins and navigation for the website.

> NOTE: Zensical is a reimplementation and extension of Material for MkDocs and MkDocs itself.


## Install Zensical

Zensical can be installed locally via vu or pip. Practicalli uses uv for simplicity.

UV Python Package Manager is used to install Zensical as a tool, negating the need for a Python Virtual environment

[UV Package Manager Install](../../programming-languages/python/uv.md){target=_blank .md-button}

Zensical commands are wrapped in Makefile tasks to provide a consistent and simple command line experience.

Install the latest version of Zensical as a tool using `uv` (updating if there is a new version).

=== "Makefile"

    ```shell
    make docs-install
    ```

    [Makefile tasks for Zensical](../../build-tool/make/zensical-tasks.md){target=_blank .md-button}


=== "Command"

    ```shell
    uv tool install zensical --with catppuccin-zensical --upgrade
    ```


## Run Zensical locally

Create a new Zensical project with `zensical new .`.

Or clone an existing repository that contains a Zensical project.

=== "Makefile"

    Build the website and serve locally at [http://localhost:7777](http://localhost:7777)

    ```shell
    make docs
    ```

=== "Command"

    Run Zensical from the root of the project, optionally specifying a host and port to run the web server hosting the site locally.

    ```shell
    zensical serve --dev-addr localhost:7777
    ```

## Additional Plugins

All plugins are defined within the `zensical.toml` configuration for the project.

[catppuccin-zensical theme](https://github.com/jonathan343/catppuccin-zensical){target=_blank} is the only plugin currently used by Practicalli projects.


## GitHub workflow

[Practicalli Workflow for Zensical static sites](../../continuous-integration/github/workflows/practicalli.md#zensical-doc-site){target=_blank .md-button}


## Reference
