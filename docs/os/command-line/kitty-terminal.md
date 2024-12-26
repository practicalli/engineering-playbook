# Kitty terminal

![Kitty Logo](https://github.com/practicalli/graphic-design/blob/live/logos/kitty-light.png?raw=true#only-dark){align=right loading=lazy style="height:150px;width:150px"}
![Kitty Logo](https://github.com/practicalli/graphic-design/blob/live/logos/kitty-dark.png?raw=true#only-light){align=right loading=lazy style="height:150px;width:150px"}

[Kitty Terminal](https://sw.kovidgoyal.net/kitty/) is a fast, feature-rich, GPU based terminal emulator providing additional features via`+kitten` extensions.

Practicalli uses Kitty as the terminal application on Linux and MacOS operating systems.  Kitty is used for running [Neovim](https://practical.li/neovim) and all command line tools.

## Install

=== "Debian Packages"
    Install Kitty via the Debian package manager.

    ```shell
    sudo apt install kitty
    ```

=== "Homebrew"
    Install the kitty homebrew package

    ```shell
    brew install --cask kitty
    ```

=== "GitHub Release"

    Download kitty from [GitHub releases](https://github.com/kovidgoyal/kitty/releases){target=_blank} or use one of the package managers for your operating system.

=== "Linux Script"

    Use the installer script to install in `~/.local/kitty.app on Linux` and `/Applications/kitty.app` on macOS.

    !!! NOTE "Install script for Linux & MacOSX"
        ```shell
        curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin \
         dest=~/.local/apps/kitty/kitty-<version>
        ```

    Add kitty and kitten to the operating system path by creating symbolic links

    !!! NOTE "Install script for Linux & MacOSX"
        ```shell
        ln -sf ~/.local/apps/kitty/kitty-<version>/bin/kitty \
        ~/.local/apps/kitty/kitty-<version>/bin/kitten \
        ~/.local/bin/
        ```

    Include kitty in desktop launchers using the kitty.desktop file

    !!! NOTE "Symbolic link for Kitty desktop application"
        ```shell
        ln -s ~/.local/apps/kitty/kitty-<version>/share/applications/kitty.desktop \
        ~/.local/share/applications/kitty.desktop
        ```

    Open text files and images in kitty via the file manager

    !!! NOTE "Symbolic link for Kitty file manager support"
        ```shell
        ln -s ~/.local/apps/kitty/kitty-<version>/share/applications/kitty-open.desktop \
        ~/.local/share/applications/kitty-open.desktop
        ```

## Configuration

Copy the installed configuration to make personal changes, or start a new configuration file in `~/.config/kitty/kitty.conf`

=== "Practicalli Kitty Config"

    Practicalli Kitty configuration is included in the Practicalli Dotfiles repository.  Clone the repository

    !!! NOTE "Clone Practicalli Dotfiles"
        ```shell
        git clone git@github.com:practicalli/dotfiles.git
        ```
        Create a symbolic link to the kitty configuration from the `$HOME/.config` directory
        ```shell
        ln -s ~/projects/practicalli/dotfiles/kitty  ~/.config/kitty
        ```

    ??? EXAMPLE "Practicalli Kitty configuration"

        ```shell title="~/.config/kitty/kitty.conf"
        --8<-- "https://github.com/practicalli/dotfiles/blob/main/kitty/kitty.conf?raw=true"
        ```

    ??? EXAMPLE "Linux specific configurations"
        Linux specific configuration is automatically loaded if OS detected as Linux
        ++ctrl++ ++shift++ ++"t"++ to open a new tab in kitty using the working directory of the current tab.
        ```config
        --8<-- "https://github.com/practicalli/dotfiles/blob/main/kitty/linux.conf?raw=true"
        ```

=== "Custom Configuration"
    Create your own custom Kitty configuration using the example configuration provided by Kitty terminal.

    !!! NOTE "Copy Default Kitty Config"
    ```shell
    cp /usr/share/doc/kitty/examples/kitty.conf ~/.config/kitty/
    ```

## Fonts

Set the font family in the `kitty.conf` configuration file

!!! EXAMPLE "Configure Kitty Font from Practicalli Dotfiles"
    ```config title="$HOME/.config/kitty/kitty.conf"
    font_family      family="FiraCode Nerd Font"
    bold_font        auto
    italic_font      auto
    bold_italic_font auto
    font_size        16
    ```

[Nerd Fonts](https://www.nerdfonts.com/) provide symbols and icons to Neovim and other terminal tools that support graphics, e.g Powerline10k command prompt themes

!!! INFO "Nerd Fonts already included in Kitty Terminal"
    [Nerd Fonts](https://www.nerdfonts.com/) do not need to be installed when using Kitty Terminal as they are already available.

??? TIP "Not Required - Manually Install Nerdfonts"
    Manually installing fonts is not required, however, for completeness this is a previous approach that was used:

    Download a Nerd Font and configure the font in `kitty.conf`

    Or download the `Symbols Nerd Font` font package for use with any font and add the Nerd Font symbols to Kitty via a symbol map configuration.

    !!! EXAMPLE "Include Nerdfont Symbols only"
        ```config title="$HOME/.config/kitty.conf"
        # Icons from NerdFont (install Nerdfont symbols only theme)
        # include ./nerdfont-icons.conf
        ```

    !!! EXAMPLE "Kitty Configuration for Nerd Fonts symbol map"
        ```shell title="$HOME/.config/kitty/nerdfont-icons.conf"
        --8<-- "https://github.com/practicalli/dotfiles/blob/main/kitty/nerdfont-icons.conf?raw=true"
        ```

## Common Key mappings

Terminal session management.

++ctrl+shift+"t"++ to create a new session in a tab window

++ctrl+shift+arrow-left++ or ++arrow-right++ to switch between tabs to the left or right

++ctrl+shift+"q"++ to close a window

++ctrl+shift+enter++ to create a new session in split window

++ctrl+shift+bracket-left++ or ++bracket-right++ to switch between window splits

++ctrl+shift+"w"++ to close a window

Other common commands include:

++ctrl+shift+equal++ to increase the font size without restarting kitty

++ctrl+shift+minus++ to increase the font size without restarting kitty

++ctrl+shift+"F11"++ to toggle kitty full-screen

++ctrl+shift+"c"++ copy from kitty terminal to clipboard

++ctrl+shift+"x"++ copy from kitty terminal to clipboard

++ctrl+shift+"v"++ paste into to kitty terminal from clipboard

++ctrl+shift+"s"++ paste into to kitty terminal from clipboard

## Kitten features

[kittens](https://sw.kovidgoyal.net/kitty/kittens_intro/) provide additional features.  Recommended features include:

* [Theme kitten](https://sw.kovidgoyal.net/kitty/kittens/themes/) - in-terminal theme browser and selector
* [diff](https://sw.kovidgoyal.net/kitty/kittens/diff/) - fast, side-by-side diff for the terminal with syntax highlighting
* [Clipboard](https://sw.kovidgoyal.net/kitty/kittens/clipboard/) - Copy/paste to the clipboard from shell scripts, even over SSH
* [SSH](https://sw.kovidgoyal.net/kitty/kittens/ssh/) - SSH with automatic shell integration, connection re-use for low latency and easy cloning of local shell and editor configuration to the remote host

## Themes

[Theme kitten](https://sw.kovidgoyal.net/kitty/kittens/themes/) provides a simple way to browse available themes and select a theme for use

Browse available themes and apply one, or ++ctrl+"c"++ to cancel

```shell
kitty +kitten themes
```

Change themes automatically to the given theme name (the theme must exist)

```shell
kitty +kitten theme theme-name
```

??? HINT "Themes used by Practicalli"
    Practicalli uses the Gruvbox Material Light Soft as the light theme
    ```shell
    kitty +kitten themes Gruvbox Material Light Soft
    ```
    Practicalli uses Gruvbox Material Dark Soft as the dark theme
    ```shell
    kitty +kitten themes Gruvbox Material Dark Soft
    ```

The first time the theme kitten is run the following config is added to the `~.config/kitty/kitty.conf` file and the chosen theme configuration written to the  `~/.config/kitty/current-theme.conf` file

This configuration shows the name of the theme, which is also in the top of the `current-theme.conf` file

```shell
# BEGIN_KITTY_THEME
# GitHub Dark
include current-theme.conf
# END_KITTY_THEME
```

Icon support from Nerd fonts, download and include the configuration to show icons in terminal based editors (e.g. Neovim, Emacs, etc.)

```shell
include ./nerdfont-icons.conf
```

## Diff

The Diff kitten provides a fast way to compare files, although there is no support for merging changes.

Kitty supports diff of image files, showing the two images side by side.

```shell
kitty +kitten diff file1 file2
```

++comma++ or ++greater-than++ jumps to next diff match

++period++ or ++less-than++  jumps to previous diff match

* [All keyboard controls](https://sw.kovidgoyal.net/kitty/kittens/diff/#keyboard-controls)

## SSH

The `+kitten` option ensures the remote environment is configured correctly for the Kitty terminal

```shell
kitty +kitten ssh hostname
```

## Image viewer

!!! NOTE "View images with kitty"
    ```shell
    kitty +kitten icat /path/to/image
    ```

create an alias in your shell configuration file, e.g. `shell-aliases`

!!! NOTE "View images with "
    ```shell
    alias icat="kitten icat"
    ```

[Kitty kitten icat](https://sw.kovidgoyal.net/kitty/kittens/icat/){target=_blank .md-button}
