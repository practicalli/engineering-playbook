# Z Shell

Z Shell (zsh) is an advanced shell environment and shell scripting language for Linux/Unix systems.

[Wikipedia - Z Shell](https://en.wikipedia.org/wiki/Z_shell){target=_blank .md-button}


## Community Configurations

Zsh is a very rich shell and a community configuration provides a quick way to use all those features.

- [Prezto](https://github.com/sorin-ionescu/prezto) with sane defaults, aliases, functions, auto completion and prompt themes
- [OhmyZsh](https://ohmyz.sh/) framework for managing your Zsh configuration with over 300 plugins

If writing your own configuration, consider [Zinit plugin manager](https://github.com/zdharma-continuum/zinit) which installs plugins from their GitHub repository.

Arch Linux has a [guide to configuring Zsh](https://wiki.archlinux.org/title/Zsh).


## Command History

++arrow-up++ and ++arrow-down++ navigate through the history of commands.

### Edit last command

Edit the previous command in the history if you know what it should be, e.g. if you typed got status instead of git statusthe you could run the command

!!! NOTE "Edit last command"
    ```shell
    fc -e astro -1
    ```

> Replace `astro` with either `emacsclient`, `nvim`, `vim`, nano or any preferred text editor.

??? EXAMPLE "Edit last command"
    Edit the `got status`to `git status` and save & exit. Now the previous command is `git status`.

    Change the `-1` to if the command is further back in history (a known location)

### Edit command history

Edit the whole command history as a file, allowing easy removal of unwanted or duplicate commands using search and replace and other editing tools.

!!! NOTE "Edit entire history"
    ```shell
    fc -W; astro "$HISTFILE"; fc -R
    ````

> Replace `astro` with either `emacsclient`, `nvim`, `vim`, `nano` or any preferred text editor.

## Shell aliases for commands

Add aliases to `$XDG_CONFIG_HOME/shell-aliases` or your preferred location of shell alias definitions.

!!! EXAMPLE "Shell Alias"
    ```shell
    # Shell history
    # edit entire history
    alias zsh-edit-history="fc -W; astro \"$HISTFILE\"; fc -R"

    # edit previous command in history
    alias zsh-edit-last-command="fc -e astro -1"
    ```

## Alternatives

- [zsh-hist plugin](https://github.com/marlonrichert/zsh-hist)

## References

- [Edit Zsh history - Stack Exchange](https://superuser.com/a/1399768)
- [zsh history setup](https://jdhao.github.io/2021/03/24/zsh_history_setup/)
