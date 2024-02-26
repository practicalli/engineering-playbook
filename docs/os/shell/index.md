# Operating System Shell

 a shell is a computer program that exposes an operating system's services to a human user or other programs. In general, operating system shells use either a command-line interface (CLI) or graphical user interface (GUI), depending on a computer's role and particular operation. It is named a shell because it is the outermost layer around the operating system.[1][2]

Command-line shells require the user to be familiar with commands and their calling syntax, and to understand concepts about the shell-specific scripting language (for example, bash), while graphical shells place a low burden on beginning computer users and are characterized as being easy to use, yet most GUI-enabled operating systems also provide CLI shells, normally for performing advanced tasks.

[Wikipedia: Shell - Computing](https://en.wikipedia.org/wiki/Shell_(computing)){target=_blank .md-button} 


## Command Line Shell


- bash
- zsh


## Aliases

Define aliases to optomise commands and create useful default flags when calling commands

Use a `shell-aliases` file to define aliases to be used with any command line shell.

!!! EXAMPLE "Shell Aliases"
    ```shell
    # Shell aliases shared across all shells (zsh, bash)

    # Neovim Aliases for multiple configurations
    alias astro="NVIM_APPNAME=astronvim nvim"

    # Neovide alias with AstroNvim configuration
    alias neovide="NVIM_APPNAME=astronvim neovide"

    # Shell history
    # edit entire history
    alias edit-shell-history="fc -W; astro \"$HISTFILE\"; fc -R"

    # edit previous command in history
    alias edit-last-command="fc -e astro -1"
    ```

Source the shell aliases from the shell configuration files

=== "Zsh"

    !!! EXAMPLE "Source Shell Aliases for Zsh"
        ```shell title=".config/zsh/.zshrc"

        # Source Shell Aliases
        [[ ! -f ~/.config/shell-aliases ]] || source ~/.config/shell-aliases
        ```

=== "Bash"

    !!! EXAMPLE "Source Shell Aliases for bash"
        ```shell title=".bashrc"
        # Source Shell Aliases
        if [ -f $HOME/.config/shell-aliases ]; then
            source $HOME/.config/shell-aliases 
        fi
        ```

## GUI Shell 

Gnome, KDE, Regolith are examples of desktop shells.
