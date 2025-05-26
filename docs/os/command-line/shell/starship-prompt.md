# Starship prompt theme

![Starship.rs Cross Shell Prompt](https://raw.githubusercontent.com/starship/starship/master/media/logo.png){align=right loading=lazy style="width:240px"}

[Starship](https://starship.rs/) is a cross-shell customizable prompt for any shell.

Make your command line more informative and more enjoyable to use.

![Starship custom prompt by Practicalli](https://github.com/practicalli/graphic-design/blob/live/os/command-line/starship-cli-preset-catppuccin-custom.png?raw=true){loading=lazy}

Starship is configured via the `~/.config/starship.toml` file.


## Install

Starship is distributed as a binary, available

=== "Debian Linux"
    Install the starshp package using the root account (or use sudo)

    ```shell
    apt install starship
    ```

=== "Homebrew"
    Install the starshp package

    ```shell
    brew install starship
    ```

=== "Arch Linux"
    Arch Linux provides starship via the extrax repository

    Install the starshp package

    ```shell
    pacman -Syu starship
    ```


## Configure Shell

Add a one-line config to [launch Starship for a specific shell](https://starship.rs/guide/#step-1-install-starship) (Bash, Fish, Zsh, etc.)

??? INFO "Switching from Prezto themes"
    Removed the prompt module theme if set in the Prezto configuration
    ```config
    # Set the prompt theme to load.
    # Setting it to 'random' loads a random theme.
    # Auto set to 'off' on dumb terminals.
    # zstyle ':prezto:module:prompt' theme 'powerlevel10k'
    ```

=== "Zsh"
    Add the starship init script at the end of the `.zshrc` file

    !!! NOTE "Starship init script for zsh"
        ```shell title=`~/.zshrc`
        eval "$(starship init zsh)"
        ```

=== "Bash"
    Add the starship init script at the end of the `.bashrc` file

    !!! NOTE "Starship init script for zsh"
        ```shell title=`~/.bashrc`
        eval "$(starship init bash)"
        ```



## Configure preset

Starship uses [presets](https://starship.rs/presets/) to configure the design of the prompt, defining what information is shown and which colors.

Starship can be used to generate a `~/.config/starship.toml`  configuration file with a specific preset.  Starship lists the preset names if the name supplied is not recognised.

Starship has a [:globe_with_meridians: Catppuccin Powerline Preset](https://starship.rs/presets/catppuccin-powerline) (theme) which complements the [Kitty Cappuccin theme]() and [Neovim Cappuccin colorscheme]()

!!! NOTE "Generate Starship config file using catppuccin-powerline preset"
    ```shell
    starship preset catppuccin-powerline -o ~/.config/starship.toml
    ```

??? EXAMPLE "Output: generate starship config file"
    ```shell-session
    ❯ starship preset catppuccin-powerline -o ~/.config/starship.toml
    error: invalid value 'catppuccin-powerline' for '[NAME]'
      [possible values: bracketed-segments, gruvbox-rainbow, jetpack, nerd-font-symbols, no-empty-icons, no-nerd-font, no-runtime-versions, pastel-powerline, plain-text-symbols, pure-preset, tokyo-night]

    For more information, try '--help'.

    NOTE:
        passed arguments: ["preset", "catppuccin-powerline", "-o", "/home/practicalli/.config/starship.toml"]
    ```

    > NOTE: Older versions of Starship may not include all of the presets listed on the website. The [`catppuccin-powerline.toml` file](https://github.com/starship/starship/blob/master/docs/public/presets/toml/catppuccin-powerline.toml) from the Starship GitHub repository was copied to `~/.config/starship.toml` file.


![Starship command line theme with catppuccin-powerline preset](https://github.com/practicalli/graphic-design/blob/live/os/command-line/kitty/kitty-starship-prompt-catppuccin-powerline-preset.png?raw=true){loading=lazy}



## Custom theme

The [:globe_with_meridians: Catppuccin Powerline Preset](https://starship.rs/presets/catppuccin-powerline) preset places all information on the left side of the prompt.  A custom version of the Cappuccin Powerline preset was created to move general information to the right hand side of the prompt and tweaked some of the default Catppuccin Mocha colors.

`format` controls the layout of the prompt.  `right_format` was added, moving the command duration, time, username and operating system details here from the left-hand side.

![Practicalli Custom version of Starship Cappuccin Powerline preset](https://github.com/practicalli/graphic-design/blob/live/os/command-line/starship-cli-preset-catppuccin-custom.png){loading=lazy}


!!! EXAMPLE "Custom Starship theme based on Cappuccin Powerline preset"
    ```toml title="~/.config/starship.toml"
    "$schema" = 'https://starship.rs/config-schema.json'

    format = """
    [](mauve)\
    $directory\
    [](fg:mauve bg:lavender)\
    $c\
    $haskell\
    $java\
    $kotlin\
    $nodejs\
    $python\
    $rust\
    [](fg:lavender bg:teal )\
    $git_branch\
    $git_status\
    [ ](fg:teal)\
    $line_break\
    $character"""

    right_format = """
    $cmd_duration\
    [](lavender)\
    $time\
    [](fg:lavender bg:mauve )\
    $username\
    [](fg:mauve bg:mauve )\
    $os\
    [ ](mauve)"""

    continuation_prompt = '▶▶ '

    palette = 'catppuccin_mocha'
    add_newline = true

    [os]
    disabled = false
    style = "bg:mauve fg:crust"

    [os.symbols]
    Windows = ""
    Ubuntu = "󰕈"
    SUSE = ""
    Raspbian = "󰐿"
    Mint = "󰣭"
    Macos = "󰀵"
    Manjaro = ""
    Linux = "󰌽"
    Gentoo = "󰣨"
    Fedora = "󰣛"
    Alpine = ""
    Amazon = ""
    Android = ""
    Arch = "󰣇"
    Artix = "󰣇"
    CentOS = ""
    Debian = "󰣚"
    Redhat = "󱄛"
    RedHatEnterprise = "󱄛"

    [username]
    show_always = true
    style_user = "bg:mauve fg:crust"
    style_root = "bg:mauve fg:crust"
    format = '[ $user]($style)'

    [directory]
    style = "bg:mauve fg:crust"
    format = "[ $path ]($style)"
    truncation_length = 5
    truncation_symbol = "…/"

    [directory.substitutions]
    "Documents" = "󰈙 "
    "Downloads" = " "
    "Music" = "󰝚 "
    "Pictures" = " "
    "Developer" = "󰲋 "

    [git_branch]
    symbol = ""
    style = "bg:teal"
    format = '[[ $symbol $branch ](fg:crust bg:teal)]($style)'

    [git_commit]
    commit_hash_length = 7
    format = '[\($hash$tag\)]($style)'
    tag_symbol = '🔖 '

    [git_status]
    style = "bg:teal"
    format = '[[($all_status$ahead_behind )](fg:crust bg:teal)]($style)'

    [nodejs]
    symbol = ""
    style = "bg:lavender"
    format = '[[ $symbol( $version) ](fg:crust bg:lavender)]($style)'

    [c]
    symbol = " "
    style = "bg:lavender"
    format = '[[ $symbol( $version) ](fg:crust bg:lavender)]($style)'

    [rust]
    symbol = ""
    style = "bg:lavender"
    format = '[[ $symbol( $version) ](fg:crust bg:lavender)]($style)'

    [golang]
    symbol = ""
    style = "bg:lavender"
    format = '[[ $symbol( $version) ](fg:crust bg:lavender)]($style)'

    [php]
    symbol = ""
    style = "bg:lavender"
    format = '[[ $symbol( $version) ](fg:crust bg:lavender)]($style)'

    [java]
    symbol = " "
    style = "bg:lavender"
    format = '[[ $symbol( $version) ](fg:crust bg:lavender)]($style)'

    [kotlin]
    symbol = ""
    style = "bg:lavender"
    format = '[[ $symbol( $version) ](fg:crust bg:lavender)]($style)'

    [haskell]
    symbol = ""
    style = "bg:lavender"
    format = '[[ $symbol( $version) ](fg:crust bg:lavender)]($style)'

    [python]
    symbol = ""
    style = "bg:lavender"
    format = '[[ $symbol( $version)(\(#$virtualenv\)) ](fg:crust bg:lavender)]($style)'

    [docker_context]
    symbol = ""
    style = "bg:sapphire"
    format = '[[ $symbol( $context) ](fg:crust bg:sapphire)]($style)'

    [conda]
    symbol = "  "
    style = "fg:crust bg:sapphire"
    format = '[$symbol$environment ]($style)'
    ignore_base = false

    [time]
    disabled = false
    time_format = "%R"
    style = "bg:lavender"
    format = '[[  $time ](fg:crust bg:lavender)]($style)'

    [line_break]
    disabled = true

    [character]
    disabled = false
    success_symbol = '[❯](bold fg:green)'
    error_symbol = '[❯](bold fg:red)'
    vimcmd_symbol = '[❮](bold fg:green)'
    vimcmd_replace_one_symbol = '[❮](bold fg:lavender)'
    vimcmd_replace_symbol = '[❮](bold fg:lavender)'
    vimcmd_visual_symbol = '[❮](bold fg:yellow)'

    [cmd_duration]
    show_milliseconds = true
    format = " in $duration "
    style = "bg:lavender"
    disabled = false
    show_notifications = false  # Desktop notifications
    min_time_to_notify = 45000

    [palettes.catppuccin_mocha]
    rosewater = "#f5e0dc"
    flamingo = "#f2cdcd"
    pink = "#f5c2e7"
    mauve = "#cba6f7"
    red = "#f38ba8"
    maroon = "#eba0ac"
    peach = "#fab387"
    yellow = "#f9e2af"
    green = "#a6e3a1"
    teal = "#94e2d5"
    sky = "#89dceb"
    sapphire = "#74c7ec"
    blue = "#89b4fa"
    lavender = "#b4befe"
    text = "#cdd6f4"
    subtext1 = "#bac2de"
    subtext0 = "#a6adc8"
    overlay2 = "#9399b2"
    overlay1 = "#7f849c"
    overlay0 = "#6c7086"
    surface2 = "#585b70"
    surface1 = "#45475a"
    surface0 = "#313244"
    base = "#1e1e2e"
    mantle = "#181825"
    crust = "#11111b"

    [palettes.catppuccin_frappe]
    rosewater = "#f2d5cf"
    flamingo = "#eebebe"
    pink = "#f4b8e4"
    mauve = "#ca9ee6"
    red = "#e78284"
    maroon = "#ea999c"
    peach = "#ef9f76"
    yellow = "#e5c890"
    green = "#a6d189"
    teal = "#81c8be"
    sky = "#99d1db"
    sapphire = "#85c1dc"
    blue = "#8caaee"
    lavender = "#babbf1"
    text = "#c6d0f5"
    subtext1 = "#b5bfe2"
    subtext0 = "#a5adce"
    overlay2 = "#949cbb"
    overlay1 = "#838ba7"
    overlay0 = "#737994"
    surface2 = "#626880"
    surface1 = "#51576d"
    surface0 = "#414559"
    base = "#303446"
    mantle = "#292c3c"
    crust = "#232634"

    [palettes.catppuccin_latte]
    rosewater = "#dc8a78"
    flamingo = "#dd7878"
    pink = "#ea76cb"
    mauve = "#8839ef"
    red = "#d20f39"
    maroon = "#e64553"
    peach = "#fe640b"
    yellow = "#df8e1d"
    green = "#40a02b"
    teal = "#179299"
    sky = "#04a5e5"
    sapphire = "#209fb5"
    blue = "#1e66f5"
    lavender = "#7287fd"
    text = "#4c4f69"
    subtext1 = "#5c5f77"
    subtext0 = "#6c6f85"
    overlay2 = "#7c7f93"
    overlay1 = "#8c8fa1"
    overlay0 = "#9ca0b0"
    surface2 = "#acb0be"
    surface1 = "#bcc0cc"
    surface0 = "#ccd0da"
    base = "#eff1f5"
    mantle = "#e6e9ef"
    crust = "#dce0e8"

    [palettes.catppuccin_macchiato]
    rosewater = "#f4dbd6"
    flamingo = "#f0c6c6"
    pink = "#f5bde6"
    mauve = "#c6a0f6"
    red = "#ed8796"
    maroon = "#ee99a0"
    peach = "#f5a97f"
    yellow = "#eed49f"
    green = "#a6da95"
    teal = "#8bd5ca"
    sky = "#91d7e3"
    sapphire = "#7dc4e4"
    blue = "#8aadf4"
    lavender = "#b7bdf8"
    text = "#cad3f5"
    subtext1 = "#b8c0e0"
    subtext0 = "#a5adcb"
    overlay2 = "#939ab7"
    overlay1 = "#8087a2"
    overlay0 = "#6e738d"
    surface2 = "#5b6078"
    surface1 = "#494d64"
    surface0 = "#363a4f"
    base = "#24273a"
    mantle = "#1e2030"
    crust = "#181926"
    ```
