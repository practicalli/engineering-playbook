# Regolith Desktop

Regolith 3 is a keyboard focused tiling window manager on top of Ubuntu or Debian, supporting X11 and Wayland (sway).

Excellent defaults make using Regolith very quick to get started and customisation is via a simple `~/.config/regolith3/Xresources` file.

[Install Guide](https://regolith-desktop.com/docs/using-regolith/install/){target=_blank .md-button}
[Basic Use](https://regolith-desktop.com/docs/using-regolith/basics/){target=_blank .md-button}
[Configuration guide](https://regolith-desktop.com/docs/using-regolith/configuration/){target=_blank .md-button}

## Install

Use a Reglolith ISO image if installing the Operating System anew.

Or add the Regolith public key and package repository to an existing Ubuntu or Debian Linux operating system.

[Regolith 3 Install Guide](https://regolith-desktop.com/docs/using-regolith/install/){target=_blank .md-button}

### Debian Linux

An additional package source is currently required to support Debian Linux Trixie (the current testing version).

> NOTE: A change to the `xorg` approach to looking up a session binary broke Regolith (and other sessions).

Update apt sources to include the unstable archive for Regolith Desktop

!!! NOTE ""
    ```shell title="/etc/apt/sources.list.d/regolith.list"
    deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg] https://archive.regolith-desktop.com/debian/testing testing main
    deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg] https://archive.regolith-desktop.com/debian/unstable testing main
    ```

??? EXAMPLE "Apt sources list for Debian Linux"

    New style apt configuration
    ```config title="/etc/apt/sources.list.d/regolith.sources"
    Types: deb
    URIs: https://archive.regolith-desktop.com/debian/testing/
    Suites: testing
    Components: main
    Signed-By: /usr/share/keyrings/regolith-archive-keyring.gpg

    Types: deb
    URIs: https://archive.regolith-desktop.com/debian/unstable
    Suites: testing
    Components: main
    Signed-By: /usr/share/keyrings/regolith-archive-keyring.gpg
    ```

## Post Install

[Regolith 3 Configuration guide](https://regolith-desktop.com/docs/using-regolith/configuration/){target=_blank .md-button}

Edit `~/.config/regolith3/Xresources` to customise Regolith (version 3)

??? EXAMPLE "Practicalli Xresources configuration"
    ```shell
    !! Postion of status bar
    wm.bar.position: top

    !! Theme
    gnome.wm.theme: Gruvbox

    !! Workspace Icons
    !! Icons to represent purpose of each workspace:
    !! issues, terminal, Clojure, db, chat, settings, music, meetings/screencats, browser
    wm.workspace.01.name: 1:<span> </span><span font_desc='FontAwesome 14'>1 </span><span foreground='#4455ff'></span><span> </span>
    wm.workspace.02.name: 2:<span> </span><span font_desc='FontAwesome 14'>2 </span><span foreground='#F7F7F7'></span><span> </span>
    wm.workspace.03.name: 3:<span> </span><span font_desc='FontAwesome 14'>3 </span><span foreground='#339e35'></span><span> </span>
    wm.workspace.04.name: 4:<span> </span><span font_desc='FontAwesome 14'>4 </span><span foreground='#f99b11'></span><span> </span>
    wm.workspace.05.name: 5:<span> </span><span font_desc='FontAwesome 14'>5 </span><span foreground='#bf1e0f'></span><span> </span>
    wm.workspace.06.name: 6:<span> </span><span font_desc='FontAwesome 14'>6 </span><span foreground='#79ace6'></span><span> </span>
    wm.workspace.07.name: 7:<span> </span><span font_desc='FontAwesome 14'>7 </span><span foreground='#d489ff'></span><span> </span>
    wm.workspace.08.name: 8:<span> </span><span font_desc='FontAwesome 14'>8 </span><span foreground='#F7F7F7'></span><span> </span>
    wm.workspace.09.name: 9:<span> </span><span font_desc='FontAwesome 14'>9 </span><span foreground='#F7F7F7'></span><span> </span>
    wm.workspace.10.name: 10:<span> </span><span font_desc='FontAwesome 14'>10 </span><span foreground='#ee8424'></span><span> </span>
    ```

### Default terminal

Set the default terminal to run when pressing ++super++ ++enter++

Open a terminal and use the update-alternatives command to list the available terminal programms.  Install the terminal app if not listed.

??? NOTE "Adminstrator access required to set default terminal"
    `sudo` is used as adminstrator rights are required to change the default terminal.  The list of available terminals can be seen without admin rights, although cannot be changed.

```shell
sudo update-alternatives --config x-terminal-emulator
```

Press the corresponding number key to select the desired terminal, e.g. ++2++ to select Kitty terminal

```shell
There are 3 choices for the alternative x-terminal-emulator (providing /usr/bin/x-terminal-emulator).

  Selection    Path                             Priority   Status
------------------------------------------------------------
* 0            /usr/bin/gnome-terminal.wrapper   40        auto mode
  1            /usr/bin/gnome-terminal.wrapper   40        manual mode
  2            /usr/bin/kitty                    20        manual mode
  3            /usr/bin/urxvt                    20        manual mode
```

!!! HINT "Practicalli recommends Kitty"
    [Kitty terminal](/engineering-playbook/command-line/kitty-terminal/) is a very easy to use and configure terminal application with great support for running Neovim and other terminal applications.

### Theme

List the available Regolith look packages to see the available themes

```shell
apt list | grep regolith-look-
```

Install the desired theme using the `regolith-look` command

```shell
regolith-look set grubox
```

Regolith Desktop should update with the new theme settings.  Use `regolith-look refresh` to force an update to the new theme settings or if further changes are made to the configuration.

## Background

Use the **Settings** app to select from the available backgrounds (if the theme doent set the desired wallpaper)
