# Zensical Static Site Generator

![Zensical Logo](https://zensical.org/assets/zensical.svg){align=right loading=lazy style="height:150px;width:150px"}

!!! QUOTE "Zensical pitch"
    A scalable Open Source systems for technical writing that always keep you in the flow

[:globe_with_meridians: Zensical](https://github.com/zensical/zensical){target=_blank} is a highly efficient tool for generating websites from Markdown content. Build times are as low as a few seconds for 500+ page site (an order of magnitude faster than most generator tools).

Content is written using Python flavoured Markdown, enabling authors to create a rich user experience without creating additional CSS or JavaScript. This website is an example of that experience.

TOML notation is used to configure a project, defining feature toggles and navigation for the website.

> NOTE: Zensical is a reimplementation of "Material for MkDocs" and MkDocs.


## Install Zensical

Zensical can be installed locally via vu or pip Python package managers. Practicalli recommends Uv for simplicity.

Uv is used to install Zensical as a tool, negating the need for a Python Virtual environment

[UV Package Manager Install](../../programming-languages/python/uv.md){target=_blank .md-button}

[Practicalli dotfiles Makefile-zensical](https://github.com/practicalli/dotfiles/blob/main/Makefile-zensical){target=_blank} defines tasks that wrap Zensical commands, providing a consistent and simple command line experience.

Install the latest version of Zensical as a tool using `uv` (updating if there is a new version).

=== "Makefile"

    ```shell
    make docs-install
    ```

    [Makefile tasks for Zensical](../../tools-services/task-runner/make/zensical-tasks.md){target=_blank .md-button}


=== "Command"

    ```shell
    uv tool install zensical --with catppuccin-zensical --upgrade
    ```


## Run Zensical locally

Create a new Zensical project, which adds a `docs` directory, `zensical.toml` config and `.github/workflow/docs.yml` to publish the site vig GitHub pages.

```shell
zensical new .
```

> Or clone an existing repository that contains a Zensical project.

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

### Custom Theme

[catppuccin-zensical theme](https://github.com/jonathan343/catppuccin-zensical){target=_blank} plugin is used to enhance the look of the Zensical theme.


!!! WARN "Overrides not compatible with theme plugin"
    Your `overrides` will not work when using a theme plugin. At the time of writing, the Zensical theme will only work with one set of `overrides`, which a theme plugin will supersede.

    A custom theme can be defined using your own `overrides` html and css files.


## GitHub workflow

[Practicalli Workflow for Zensical static sites](../../tools-services/continuous-integration/github/workflow/practicalli.md#zensical-static-site-generator){target=_blank .md-button}
