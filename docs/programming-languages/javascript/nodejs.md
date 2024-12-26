# Node.js

Node.js is an environment to run JavaScript on the operating system, without the need for a browser.

Node.js is useful for running JavaScript tools and server-side services.

## Install

=== "Debian/Ubuntu"
    The nodejs package available in Ubuntu required a scary amount of dependencies, so it may be preferable to install nodejs locally, in your own account.  This does mean node is only available to this specific user account.

    ??? WARNING "Avoid nodejs debian packages when using Neovim Mason"
        Mason is a Neovim plugin to manage installation of LSP servers, format and lint tools.

        The debian packages potentially cause issues with Mason updates for specific format and lint tools.

        Install Node.js via the Linux archive available from the [Nodejs Website - Download](https://nodejs.org/){target=_blank} 

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

=== "MacOSX Homebrew"

    Install using brew.sh and the [Homebrew Formulae for node](https://formulae.brew.sh/formula/node)

    ```shell
    brew install node
    ```
