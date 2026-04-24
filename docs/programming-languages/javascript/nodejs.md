# Node.js

Node.js is an environment to run JavaScript without the need for a web browser.

Node.js is useful for running JavaScript command line tools and server-side services.

The Nodejs website provides [an install script tool that can use NVM, FNM, Docker or Homebrew](https://nodejs.org/en/download){target=_blank} to install any version of Node.js.  Scripts are provided for Linux, MacOS and Windows operating systems.


## Install

Practicalli created a script to install Node.js via FNM on Linux. FNM is a fast and simple version manager for Node.js and adds itself to the system executable path.

=== "Practicalli Script"


    The install script is an extension of the Node.js install script, adding support for XDG_CONFIG_HOME and including a check of the Node.js version installed.

    !!! "Install Nodejs via FNM"
        ```shell
        #!/usr/bin/env bash

        # NOTE: run this script after setting up preferred shell, e.g. zsh
        # as the NVM script adds itself to the shell resource configuration

        echo
        echo "# ---------------------------------------"
        echo "Nodejs install via Node Version Manager - FNM"

        # Use argument as node version or use "24" if no argument passed
        nodeversion="${1:-24}"

        # Download and install fnm:
        # https://github.com/Schniz/fnm
        curl -o- https://fnm.vercel.app/install | bash

        # prevent duplication in your shell config file, pass --skip-shell to the install command:
        # curl -fsSL https://fnm.vercel.app/install | bash -s -- --skip-shell

        # Define location of FNM
        if [ -d "$XDG_CONFIG_HOME"/fnm ]; then
          echo "Source FNM script from XDG_DATA_HOME/fnm"
          FNM_PATH="$XDG_DATA_HOME/fnm"
        else
          echo "Source NVM script from HOME/.local/share/fnm"
          FNM_PATH="$HOME/.local/share/fnm"
        fi

        # Source the FNM script
        if [ -d "$FNM_PATH" ]; then
          export PATH="$FNM_PATH:$PATH"
          # eval "`fnm env`"  # shellcheck reports bad form
          eval "$(fnm env)"
        fi

        # Download and install Node.js:
        fnm install "$nodeversion"

        # Verify the Node.js version:
        echo "Node.js version: $(node -v)"

        # Verify npm version:
        echo "NPM version: $(npm -v)"
        echo "# ---------------------------------------"
        echo
        ```

        Run the script to install Node.js version 24.

        !!! "Install Nodejs version 24"
        ```shell
        ./nodejs-fnm-install.sh
        ```

        Pass an argument to the script to install a specific version of Node.js

        !!! "Install Nodejs version 25"
        ```shell
        ./nodejs-fnm-install.sh 25
        ```

        !!! INFO "Practicalli Debian Linux Post Install scripts"
            [Practicalli Dotfiles repository](https://github.com/practicalli/dotfiles){target=_blank} contains scripts to install a range of software development tools, TUI's, Programming Launguages and Linux system administration tools.

=== "MacOSX Homebrew"

    Install via [brew.sh](https://brew.sh/) and the [Homebrew Formulae for node](https://formulae.brew.sh/formula/node)

    ```shell
    brew install node
    ```

=== "Manual Install"

    [Nodejs Website - Download](https://nodejs.org/){target=_blank .md-button}

    [Download the relevant version of nodejs from the website](https://nodejs.org/), either the Long Term Support version (recommended) or the latest release

    Create a suitable directory path to extract the downloaded nodejs archive to

    ```shell
    $HOME/.local/apps/nodejs/
    ```

    Extract the downloaded nodejs archive into this path

    ```shell
    tar -Jvxf node-v20.7.0-linux-x64.tar.xz -C $HOME/.local/apps/nodejs/
    ```
    > The `tar` flag `-J` uses the XZ compression algorithm, `v` for verbose list of extracted files, `x` to extract all files, `f` to specify file (or remote file, tape drive)


    Create a symbolic link called current that points to the top level directory of the nodejs archive just unpacked, e.g. node-v<specific-version-number>
    ```
    ln -s node-v20.7.0-linux-x64 current
    ```

    Add nodejs directory to the execution path, so that any additional node executables will automatically be included

    Edit the `~/.zshenv` file, adding a conditional path entry on the nodejs directory existing

    ```shell
    ## Nodejs local install
    if [[ -d $HOME/.local/apps/nodejs/current ]]; then
      path=($HOME/.local/apps/nodejs/current/bin(/N) $path)
    fi
    ```
    This example adds the nodejs bin directory at the start of the current executable path.

    Use `path+=($HOME/.local/apps/nodejs/current/bin(/N))` instead to append the nodejs bin directory to the end of the PATH.

    Open a new terminal to pick up the change or source the `.zshenv` file in an existing terminal

    ```shell
    source ~/.zshenv
    ```

=== "Debian Packages"

    The stable release of Debian Linux, Trixie, packiages Node.js version 20.

    Node.js and Node Package Manager (NPM) are packaged separately.

    !!! NOTE "Install Node.js and NPM packages"
    ```shell
    apt install nodejs npm
    ```

    > NOTE: Debian Linux nodejs package has a large amount of dependencies. Practicalli recommends install Nodejs locally, in your own account.  This does mean node is only available to this specific user account.
