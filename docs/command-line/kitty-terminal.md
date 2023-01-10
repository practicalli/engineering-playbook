# Kitty terminal

[Kitty Terminal](https://sw.kovidgoyal.net/kitty/) is a fast, feature-rich, GPU based terminal emulator

## install


=== "Ubuntu/Debian"

    ```shell
    sudo apt install kitty
    ```

=== "Homebrew"

    ```shell
    brew install --cask kitty
    ```

Copy the installed configuration to make personal changes, or start a new configuration file in `~/.config/kitty/kitty.conf`

```shell
cp /usr/share/doc/kitty/examples/kitty.conf ~/.config/kitty/
```


## Common configuration options

```shell
font_family FiraCode
font_size 16
```

## Multiple sessions

++ctrl+shift+"T"++ to create a new session in a tab window

++ctrl+shift+arrow-left++ or ++arrow-right++ to switch between tabs to the left or right

++ctrl+shift+enter++ to create a new session in split window

++ctrl+shift+bracket-left++ or ++bracket-right++ to switch between window splits


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


* [All keyboard controls](fast, feature-rich, GPU based terminal emulator)



## SSH

The `+kitten` option ensures the remote environment is configured correctly for the Kitty terminal

```shell
kitty +kitten ssh hostname
```


## Example configuration

Practicalli uses the following configuration for Kitty terminal across multiple operating systems

```shell title="~/.config/kitty/kitty.conf"
# ---------------------------------------------------------
# Practicalli Kitty terminal theme
#
# Configuration using GitHub theme with light and dark options
# using FiraCode font and NerdFont symbol mappings for icon support
# for powerline10k and web-devicons in Neovim
# ---------------------------------------------------------

# ---------------------------------------------------------
# Colorscheme / Icons

# Icons from NerdFont (install Nerdfont symbols only theme)
include ./nerdfont-icons.conf

# `kitty +kitten theme` to browse available themes and apply one
# `kitty +kitten theme theme-name` to change themes automatically
# Favorite themese include:
# Catppuccin-Latte and Catppuccin-Mocha
# GitHub Light and GitHub Dark

# BEGIN_KITTY_THEME
# GitHub Dark
include current-theme.conf
# END_KITTY_THEME
# ---------------------------------------------------------

# ---------------------------------------------------------
# Feedback
enable_audio_bell no
# visual_bell_color none
# ---------------------------------------------------------

# ---------------------------------------------------------
# Tab styles
# fade slant separator powerline custom hidden
tab_bar_style powerline
tab_bar_align left
tab_powerline_style angled
# ---------------------------------------------------------

# ---------------------------------------------------------
# Fonts
font_family FiraCode

# Patched fonts (not recommended for Kitty)
# font_family MesloLGS NF
# font_family Fira Code NF

# bold_font        auto
# italic_font      auto
# bold_italic_font auto
font_size 14

# adjust_line_height  0
# adjust_column_width 0
# adjust_baseline 0
# ---------------------------------------------------------
```
