# UV Python Package Manager

[UV](https://docs.astral.sh/uv/){target=_blank} is an extremely fast Python package manager written in Rust, designed to replace tools like pip and pip-tools.

UV can manage project dependencies, creating Python virtual environments, and executing command-line tools.

!!! TIP "Uv tool is a very simple way to install python tools"

## Install

Install UV via the provided install script, or from the latest GitHub release using [DNA]()

> NOTE: UV website contains [additional install options](https://docs.astral.sh/uv/getting-started/installation/).


=== "Install Script"

    !!! NOTE "UV install Script"
        ```shell
        curl -LsSf https://astral.sh/uv/install.sh | sh
        ```

    > NOTE: review any script before running it locally


=== "GitHub Release (DNA)"

    Install UV from the latest GitHub Release, using [DRA](https://github.com/devmatteini/dra){target=_blank}.

    The UV GitHub Release is packages as a `.tar.gz` file that includes `uv` and `uvx` executables.  Both of these executables should be installed to use UV completely.

    The DRA tool by default extracts a single file when installing from a tar or zip archive.

    DRA can install multiple executables from a tar or zip archive using the `-I`/ `--install-file` option.  The option should be specified for each file to be extracted


    !!! EXAMPLE "Bash script to install UV latest release"
        ```script
        #!/usr/bin/env bash

        echo "# ---------------------------------------"
        echo "UV python package manager"
        dra download --automatic --install-file uv --install-file uvx --output ~/.local/bin/ astral-sh/uv

        echo
        echo "Uv version: $(uv --version)"
        echo "# ---------------------------------------"
        echo
        ```
