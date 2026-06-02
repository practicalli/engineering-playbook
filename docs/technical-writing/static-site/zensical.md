# Zensical Static Site Generator

[:globe_with_meridians: Zensical](https://github.com/zensical/zensical){target=_blank} is a Rust & Python tools for generating documentation sites from Markdown (potentially Common Mark in future).

UV Python Package Manager is used to [install Zensical as a tool](#install-zensical), negating the need for a Python Virtual environment

`zensical.toml` is used to configure a project, defining plugins and navigation for the website.

> NOTE: Zensical is a reimplementation and extension of Material for MkDocs and MkDocs itself.


## Zensical Tasks

Practicalli defines tasks in a Makefile to manage every Zensical project.



!!! EXAMPLE "Makefile tasks for Zensical using UV"
    ```make
    # -- Makefile Variables -------------------------- #
    DOCS_SERVER := zensical serve --dev-addr localhost:7777
    # ------------------------------------------------ #

    # --- Documentation Generation  ------------------ #
    docs-install:  ## Install or upgrade Zensical in Python virtual environment
    	uv tool install zensical --upgrade

    docs:  ## Build and run docs in local server
    	$(info -- Local Server --------------------------)
    	$(DOCS_SERVER)

    docs-open:  ## Build docs, run server & open browser
    	$(info -- Local Server & Browser ----------------)
    	$(DOCS_SERVER) --open

    docs-build:  ## Build docs locally
    	$(info -- Build Docs Website --------------------)
    	zensical build

    docs-debug:  ## Run local server in debug mode
    	$(info -- Local Server Debug --------------------)
    	$(DOCS_SERVER) -v

    dist: docs-build ## Build mkdocs website
    # ------------------------------------------------ #
    ```


## Install Zensical

Zensical can be installed locally via vu or pip. Practicalli uses uv for simplicity.  Commands are wrapped in tasks defined within the `Makefile`.

[UV Package Manager Install](/programming-languages/python/uv.md){target=_blank .md-button}

=== "Makefile"

    ```shell
    make docs-install
    ```

=== "Command"

    Install Zensical as a tool using `uv` (updating if there is a new version).

    ```shell
    uv tool install zensical --upgrade
    ```


## Run Zensical locally

Create a new Zensical project with `zensical new .`.

Or clone an existing repository that contains a Zensical project.

=== "Makefile"

    Build the website and serve locally at [http://localhost:8000](http://localhost:8000)

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


## GitHub workflow

[Practicalli Workflow for Zensical static sites](/continuous-integration/github/workflows/practicalli.md#zensical-doc-site){target=_blank .md-button}


## Reference
