# Kitty terminal

![Kitty Logo](https://raw.githubusercontent.com/practicalli/graphic-design/live/icons/kitty-light.png#only-dark){align=right loading=lazy style="height:150px;width:150px"}
![Kitty Logo](https://raw.githubusercontent.com/practicalli/graphic-design/live/icons/kitty-dark.png#only-light){align=right loading=lazy style="height:150px;width:150px"}

[Kitty Terminal](https://sw.kovidgoyal.net/kitty/) is a fast, feature-rich, GPU based terminal emulator providing additional features via`+kitten` extensions.

## Install


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


## Fonts

[Nerd Fonts](https://www.nerdfonts.com/) are recommended to provide icon support in Kitty

Download a Nerd Font and configure the font in `kitty.conf`

!!! EXAMPLE "Kitty Font and Font Family"
    ```shell
    font_family FiraCode Nerd Font
    font_size 16
    ```

Or download the `Symbols Nerd Font` font package for use with any font and add the Nerd Font symbols to Kitty via a symbol map configuration.

??? EXAMPLE "Kitty Configuration for Nerd Fonts symbol map"
    ```shell title="$HOME/.config/kitty/nerdfont-icons.conf"
    # ---------------------------------------------------------
    # NerdFont icons via symbol maps
    #
    # Kitty recommends mapping symbols rather than using patched fonts
    #
    # Download symbols only font from
    # <https://github.com/ryanoasis/nerd-fonts/blob/master/src/glyphs/Symbols-2048-em%20Nerd%20Font%20Complete.ttf>
    #
    # List available fonts with:
    #   kitty +list-fonts
    #
    # Troubleshoot missing/incorrect characters with:
    #   kitty --debug-font-fallback
    #
    # Reference: <https://erwin.co/kitty-and-nerd-fonts/>

    # Symbols Nerd Font - complete symbol_map
    
    # "Nerd Fonts - Pomicons"
    symbol_map  U+E000-U+E00D Symbols Nerd Font
    
    # "Nerd Fonts - Powerline"
    symbol_map U+e0a0-U+e0a2,U+e0b0-U+e0b3 Symbols Nerd Font
    
    # "Nerd Fonts - Powerline Extra"
    symbol_map U+e0a3-U+e0a3,U+e0b4-U+e0c8,U+e0cc-U+e0d2,U+e0d4-U+e0d4 Symbols Nerd Font
    
    # "Nerd Fonts - Symbols original"
    symbol_map U+e5fa-U+e62b Symbols Nerd Font
    
    # "Nerd Fonts - Devicons"
    symbol_map U+e700-U+e7c5 Symbols Nerd Font
    
    # "Nerd Fonts - Font awesome"
    symbol_map U+f000-U+f2e0 Symbols Nerd Font
    
    # "Nerd Fonts - Font awesome extension"
    symbol_map U+e200-U+e2a9 Symbols Nerd Font
    
    # "Nerd Fonts - Octicons"
    symbol_map U+f400-U+f4a8,U+2665-U+2665,U+26A1-U+26A1,U+f27c-U+f27c Symbols Nerd Font
    
    # "Nerd Fonts - Font Linux"
    symbol_map U+F300-U+F313 Symbols Nerd Font
    
    #  Nerd Fonts - Font Power Symbols"
    symbol_map U+23fb-U+23fe,U+2b58-U+2b58 Symbols Nerd Font
    
    #  "Nerd Fonts - Material Design Icons"
    symbol_map U+f500-U+fd46 Symbols Nerd Font
    
    # "Nerd Fonts - Weather Icons"
    symbol_map U+e300-U+e3eb Symbols Nerd Font
    
    # Misc Code Point Fixes
    symbol_map U+21B5,U+25B8,U+2605,U+2630,U+2632,U+2714,U+E0A3,U+E615,U+E62B Symbols Nerd Font
    # ---------------------------------------------------------
    ```


## Multiple sessions

Create a new terminal session without leaving kitty by creating a new tab or a window split.

++ctrl+shift+"T"++ to create a new session in a tab window

++ctrl+shift+arrow-left++ or ++arrow-right++ to switch between tabs to the left or right

++ctrl+shift+"Q"++ to close a window

++ctrl+shift+enter++ to create a new session in split window

++ctrl+shift+bracket-left++ or ++bracket-right++ to switch between window splits

++ctrl+shift+"W"++ to close a window


Other common commands include:

++ctrl+shift+equal++ to increase the font size without restarting kitty

++ctrl+shift+minus++ to increase the font size without restarting kitty

++ctrl+shift+"F11"++ to toggle kitty full-screen

++ctrl+shift+"c"++ copy from kitty terminal to clipboard

++ctrl+shift+"v"++ paste into to kitty terminal from clipboard

++ctrl+shift+"s"++ paste into to kitty terminal from clipboard
Paste from Selection Ctrl+Shift+S


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
