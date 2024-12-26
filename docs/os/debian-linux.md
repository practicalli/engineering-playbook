# Debian Linux

Debian Linux provides support for the widest range of hardware of any Linux distribution.

The Debian project established the high-quality `.deb` package system which ensures all packages are consitenly defined and manage dependencies.

## Install

Debian provides ISO images which can be burned onto compact disks or USB memory sticks.

The net install image is small and quick to download, containing only the essential packages to run an operating system.

Copy the Debian Linux ISO image to a USB of 1GB size or larger

!!! EXAMPLE "Create USB install disk from Debian ISO file"
    ```shell
    cp debian.iso /dev/sda
    ```

??? HINT "Creating USB install disks with non-Debian images"
    Find the name of the USB stick
    ```shell
    ls -l /dev/disk/by-id/usb-*
    ```

    Copy the image using the name of the USB Stick
    ```shell
    dd bs=4M if=path/to/filename.iso of=/dev/disk/by-id/usb-My_flash_drive conv=fsync oflag=direct status=progress
    ```
    [Arch Linux: ISO image command line utilities](https://wiki.archlinux.org/title/USB_flash_installation_medium)

## Post Install

### root vs sudo

Debian recommends using the root account to administer the system, rather than using `sudo` as with Ubuntu

`su -` command in a terminal changes to the root user, upon entering a successful root password at the prompt

```shell
su -
```

> root password is set during install of Debian

!!! HINT "Dedicated terminal for root account"
    Open a terminal application specifically to use the root account when carrying out significant maintenance, e.g. installing many packages during the post install.

## Set XDG freedesktop locations

=== "Zsh"

    ```config
    # Set XDG_CONFIG_HOME for clean management of configuration files
    export XDG_CONFIG_HOME="${XDG_CONFIG_HOME:=$HOME/.config}"
    export XDG_DATA_HOME="${XDG_DATA_HOME:=$HOME/.local/share}"
    export XDG_STATE_HOME="${XDG_STATE_HOME:=$HOME/.local/state}"
    export XDG_CACHE_HOME="${XDG_CACHE_HOME:=$HOME/.cache}"
    export ZDOTDIR="${ZDOTDIR:=$XDG_CONFIG_HOME/zsh}"
    ```

=== "Bash"

## Git install

```shell
apt install git
```

## Clone practicalli/dotfiles

clone to projects/practicalli/dotfiles

## Git configure

link .config/git to Practicall dotfiles/git directory

```shell
ln -s ~/projects/practicalli/dotfiles/git ~/.config/git
```

Update identity-practicalli-john file with annonymous email address from GitHub settings > Emails section

## Kitty Terminal

Debian packages

- kitty
- kitty-doc
- kitty-shell-integration
- kitty-terminfo
- `fonts-firacode` use by Practicalli Kitty terminal

```shell
apt install kitty kitty-doc kitty-shell-integration kitty-terminfo fonts-firacode

```

Link to practicalli/dotfiles/kitty in .config directory

```shell
ln -s ~/projects/practicalli/dotfiles/kitty ~/.config/kitty
```

## Zsh

Install zsh package

```shell
apt install zsh
```

## Zsh Configuration

[Prezto]() and [OhMyZsh]() are community created configurations that provide a rich experience with very little work.

!!! HINT "Prezto recommended"
    Practicalli recommends Prezto as it has excellent completion capabilities, especially wish fish mode enabled.

    OhMyZsh is used within Termux as Prezto has not worked correctly in my experiences.

=== "Prezto"

    Prezto provides sane defaults, aliases, functions, auto completion, and prompt themes.


    As the user account, `zsh` command to change to zsh from bash shell

    check if XDG_CONFIG_HOME is set, if not set to $HOME/.config on command line (practicalli dotfiles has a zshenv that does this later in process)

    clone prezto to XDG location

    !!! EXAMPLE "Clone Prezto to XDG_CONFIG_HOME"
    ```shell
    git clone --recursive https://github.com/sorin-ionescu/prezto.git "${ZDOTDIR:-${XDG_CONFIG_HOME:-$HOME/.config}/zsh}/.zprezto"
    ```


    ```shell
    export XDG_CONFIG_HOME="${XDG_CONFIG_HOME:=$HOME/.config}"
    [[ -d $XDG_CONFIG_HOME/zsh ]] && export ZDOTDIR="$XDG_CONFIG_HOME/zsh"
    source "$ZDOTDIR/.zshenv"
    ```

    Generate symbolic links to the zsh configuration files residing in the prezto directory

    ```shell
    setopt EXTENDED_GLOB
    for rcfile in "${ZDOTDIR:-$HOME}"/.zprezto/runcoms/^README.md(.N); do
      ln -s "$rcfile" "${ZDOTDIR:-$HOME}/.${rcfile:t}"
    done
    ```

    Practicalli creates custom configuration files to keep updating Prezto a simple Git pull.

    - `.zprezto` practicalli customisations for Prezto
    - `.zprofile` includes .local/bin on execution path
    - `.zshrc` aliases for neovim configurations (astro, practicalli
    - `.zshenv` configures XDG locations

    ```shell
    .zprezto
    .p10k.zsh
    .zlogin -> /home/practicalli/.config/zsh/.zprezto/runcoms/zlogin
    .zlogout -> /home/practicalli/.config/zsh/.zprezto/runcoms/zlogout
    .zpreztorc -> /home/practicalli/projects/practicalli/dotfiles/zsh/.zpreztorc
    .zprofile -> /home/practicalli/.config/zsh/.zprezto/runcoms/zprofile
    .zshenv -> /home/practicalli/projects/practicalli/dotfiles/zsh/.zshenv
    .zsh_history
    .zshrc -> /home/practicalli/projects/practicalli/dotfiles/zsh/.zshrc
    ```

    !!! EXAMPLE "Change the default shell"
    ```shell
    chsh -s /usr/bin/zsh
    ```

## Neovim

!!! HINT "Practicalli Neovim install"
    See the [install guide in Practicalli Neovim](https://practical.li/neovim/install/) book

Required packages

- `curl` - suggested by neovim checkhealth
- `gdu`
- `xclip` provider for accessing operating system clipboard from Neovim
- `luarocks` required for some mason installed tools
- `lazygit` terminal UI git client used by AstroNvim

```shell
apt install curl gdu lazygit luarocks xclip
```

> `btm` is suggested but not available as a Debian packages

### Nodejs

Nodejs is required for many of the linters installed by Mason.

`node` and `npm` are available as packages, although the `npm` package has a great many dependencies (many of which seem redundant).

Download from nodejs website

Extract to `~/.local/apps/nodejs`

Create a symbolic link to make it simpler to use alternate versions of node and npm

```shell
ln -s ~/.local/apps/nodejs/node-v18.17.1-linux-x64/ ~/.local/apps/nodejs/current
```

Create symbolic links for `node` and `npm` to add them to the OS excecution path

```shell
ln -s ~/.local/apps/nodejs/current/bin/node ~/.local/bin/node &&
ln -s ~/.local/apps/nodejs/current/bin/npm ~/.local/bin/npm &&
```

If a different version of nodejs is installed, then all that should be required is deleting and recreating the `current` symbolic link in `~/.local/apps/nodejs/` to point to the desired version.

### Releases

Download latest appimage for neovim from Neovim repository releases page

!!! EXAMPLE "Make Neovim executable"
    ```shell
    chmod u+x nvim.appimage
    ```

!!! EXAMPLE "Add Neovim to shell execution path"
    ```shell
    mv nvim.appimage ~/.local/bin && \
    ln -s ~/.local/bin/nvim.appimage ~/.local/bin/nvim
    ```

Edit `.config/zsh/.zprofile` and add `$HOME/.local/bin` to paths to search for executables

Set the list of directories that Zsh searches for programs.

!!! EXAMPLE "Zsh paths searched for executable files"
    ```
    path=(
      $HOME/.local/{,s}bin(N)
      /opt/{homebrew,local}/{,s}bin(N)
      /usr/local/{,s}bin(N)
      $path
    )
    ```

Use the `source` command to load the changes in `.zprofile` to  update the path and include the `.local/bin` path

!!! EXAMPLE "Load configuration into current shell session"
    ```shell
    source .config/zsh/.zprofile
    ```

### Configure nvim

Clone astronvim configuration and Practicalli Astronvim configuration

Add aliases to `.zshrc` to call different neovim configurations

!!! EXAMPLE "Shell Aliases for neovim multiple configurations"
    ```shell
    alias astro="NVIM_APPNAME=astronvim nvim"
    alias lazyvim="NVIM_APPNAME=lazyvim nvim"
    alias practicalli-redux="NVIM_APPNAME=neovim-config-redux nvim"
    ```

[Practicalli Neovim - Multiple Configurations](https://practical.li/neovim/install/multiple-configurations/){target=_blank .md-button}

## Date and Time

The `date` command on Linux systems is used to show the system date or set a specific date and time. The system date can only be changed by the root account or accounts in the `sudo` group.

When using the Gnome desktop the system date is automatically managed, keeping the date and time current.

The `timedatectl` command is used to control automatic updating of the system time.  This must be disabled to set the date and time to something other than the current.

!!! NOTE "Disable automatic date-time"
    ```shell
    timedatectl set-ntp 0
    ```

When the timedateclt is disabled, then the `date` command can be used to set a specific date and or time.

!!! EXAMPLE "Set the date and time"
    ```shell
     date -s '2024-09-16 21:32:00'
    ```

`date` command will show the current date, confirming that the OS system date was changed.

!!! NOTE "Enable automatic date-time"
    ```shell
    timedatectl set-ntp 1
    ```

> Linux used to use the ntp service which is available via the Debian `ntp` package, but not used by Gnome desktop

## Add Sid packages on Testing

Packages can be installed from `sid` (unstable) when the `testing` version of Debian Linux is installed, referred to as "Testing-Unstable Mix".

Configure apt to ensure a testing system stays on testing, without apt upgrading every package to the unstable version.

Define testing as the default Debian release

!!! EXAMPLE "Set Default Release as testing with security updates"
    ```config title="/etc/apt/apt.conf.d/20-tum.conf"
    APT::Default-Release "/^testing(|-security|-updates)$/";
    ```

In the Apt sources.list configuration, copy the main testing line and change the version to unstable

!!! EXAMPLE "Add unstable to package sources"
    ```conf title="/etc/apt/sources.list"
    # Testing
    deb <http://deb.debian.org/debian/> trixie main contrib non-free-firmware non-free
    deb-src <http://deb.debian.org/debian/> trixie main contrib non-free-firmware non-free
    deb <http://deb.debian.org/debian/> trixie-updates main contrib non-free-firmware non-free
    deb <http://deb.debian.org/debian/> trixie-backports main contrib non-free-firmware non-free

    deb http://security.debian.org/debian-security trixie-security main contrib non-free-firmware
    deb-src http://security.debian.org/debian-security trixie-security main contrib non-free-firmware

    # Sid
    # deb http://deb.debian.org/debian/ unstable main contrib non-free-firmware non-free
    ```

Run apt update to refresh the cache. Use `apt -t unstable install <package-name>` to install package foo from unstable rather than testing.

!!! EXAMPLE "Install specific package from Sid"
    ```shell
    apt -t unstable install hyprland



## Debian Tracker

[Tracker service](https://wiki.ubuntu.com/Tracker) indexes many types of files to enable discovery of files by other Gnome services and applications.

- [desktop search](https://wiki.ubuntu.com/IntegratedDesktopSearch)
- Tag database for keyword tagging
- Extensible metadata database to add custom metadata to files, e.g. rhythmbox, gedit, etc.
- Store First Class Objects and the Gnome 3.0 Model

> NOTE: when actively using Gnome desktop and Gnome apps, disabling the tracker may reduce functionality


### Disable the tracker service

The tracker service can be a significant drain on computer resources as it indexes files, especially when there have been a log of changes or for a newly installed system.

The tracker has many dependencies, so its not easy to remove the `tracker-miner-fs-3` package when actively using the Gnome desktop.

The recommended approach is to edit the `.desktop` files and add `Hidden=true` at the end of each tracker related file and reboot the operating system.

```config title="/etc/xdg/autostart/tracker-miner-fs-3.desktop"
[Desktop Entry]
Name=Tracker File System Miner
Comment=Crawls and processes files on the file system
Exec=/usr/libexec/tracker-miner-fs-3
Terminal=false
Type=Application
Categories=Utility;
X-GNOME-Autostart-enabled=false
X-GNOME-HiddenUnderSystemd=false
# X-KDE-autostart-after=panel
X-KDE-StartupNotify=false
X-KDE-UniqueApplet=true
NoDisplay=true
OnlyShowIn=GNOME;KDE;XFCE;X-IVI;Unity;
X-systemd-skip=true
Hidden=true
```

If adding "Hiddent=true" is not sufficient, then disable the services for all users by setting them to `/dev/null` using the `systemctl` command.

```shell
sudo systemctl --global mask tracker-miner-fs-3.service
sudo systemctl --global mask tracker-xdg-portal-3.service
```

Remove the database of indexed files from each user account on the system

```shell
rm -rf $HOME/.cache/tracker*
```
```
