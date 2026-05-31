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


=== GitHub Release (DNA)

    Install UV from the latest GitHub Release, using [DRA]().

    !!! EXAMPLE "Bash script to install UV latest release"
        ```script
        #!/usr/bin/env bash

        echo "# ---------------------------------------"
        echo "UV python package manager"
        dra download --automatic --install --output ~/.local/bin/ astral-sh/uv

        echo
        echo "Uv version: $(uv --version)"
        echo "# ---------------------------------------"
        echo
        ```
