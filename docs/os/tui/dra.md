# DRA Download Release Assets

A command line tool to download release assets from GitHub, without the need for GitHub authentication.

DRA can download and install the latest release for the current operating system.

![DRA in action](https://github.com/devmatteini/dra/raw/main/assets/demo.gif){loading=lazy style="height:150px;width:150px"}


=== "Install Script"


!!! NOTE "DRA install to $HOME/.local/bin"
    ```shell
    curl --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/devmatteini/dra/refs/heads/main/install.sh | bash -s -- --to $HOME/.local/bin
    ```

    > NOTE: review script before installing locally.


=== "Practicalli Script"

    A script to install DRA globally (/usr/local/bin/) and generate

!!! EXAMPLE ""
    ```shell
    #!/usr/bin/env bash

    # Instal DRA via its own install script,
    # which can also be used to update DRA.
    #
    # DRA cannot replace itself as running binaries can not be altered

    echo
    echo "# ---------------------------------------"
    echo "DRA - Download Release Assests from GitHub"
    curl --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/devmatteini/dra/refs/heads/main/install.sh | sudo bash -s -- --to /usr/local/bin

    # Generate shell command completion
    if [[ $SHELL == "/bin/bash" ]]; then
      mkdir -p ~/.local/share/bash-completion/completions/
      dra completion bash >~/.local/share/bash-completion/completions/dra

    elif [[ $SHELL == "/usr/bin/zsh" ]]; then
      mkdir -p ~/.local/share/zsh-completion
      dra completion zsh >~/.local/share/zsh-completion/_dra
    else
      echo "Unknown SHELL, Ripgrep completions will not be generated"
      exit
    fi

    echo
    echo "Dra version: $(dra --version)"
    echo "# ---------------------------------------"
    echo
    ```
