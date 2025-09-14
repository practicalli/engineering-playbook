# Hyprland Tiling Compositor

Hyprland is a high performance Wayland compositor and tiling window manager.  Hyprland is not a full desktop environment (DE) like [KDE Plasma](https://kde.org/) or [Gnome](https://www.gnome.org/).

Hyprland is not a full desktop environment, unlike KDE or Gnome. There are distributions that provide a complete Hyprland based desktop environment.

- Omachy: simple and opinionated distribution from Arch Linux & Hprland, completely keyboard driven and providing a crafted experience.
- Garuda Linux: hyprlant variant provides a professional and easy to manage operating system, G-Hyprland provides optional extra Hyprland customisation.
- NWG Live

Hyprland can be installed on Arch Linux and Nix distributions.  Community scripts are available for Debian and RedHat based distributions.


!!! WARNING "Hyprland can be very time consuming"

## Install Options

!!! WARNING "Hyprland is a quickly evolving project and issues may occur"

=== "Omarchy"

    [Omarchy](https://omarchy.org/) is a Linux distribution that provides a keyboard driven and unified menu experience, building on Arch Linux and Hyprland (Wayland compositor and tiling window manager).

    Omachy is driven through its menu and configuration files are opened in Neovim (LazyVim config).

    Google Mail and other web apps are opened as headless browsers, for a highly focused experience.

    A base install option allows for a highly customised opeating system, whilst still retaining the simplicity of Omachy.


=== "Garuda Linux"

    [Garuda Linux](https://garudalinux.org/) is a distribution on top of Arch Linux.  There are several variants which includes one centered on Hyprland.

    Garuda Linux Hyprland is recommended when first starting (especially if not used to Arch Linux).

    Garuda uses the BtrFs file system which allows snapshots and rollbacks to recover the operating system and desktop should hyprland or arch linux pakcages break.


=== "NWG Live ISO"

    [NWG-ISO](https://github.com/nwg-piotr/nwg-iso) is a simple way to try out hyprland and the [NWG Shell tools](https://nwg-piotr.github.io/nwg-shell/), providing an experience close to a Desktop Environment like Gnome or KDE.

    NWG Live ISO will run from memory.  Once running, the Live ISO can also be used to install Hyprland & NWG tools on the permanent storage of the computer.

    ![NWG Shell login greeter](https://github.com/nwg-piotr/nwg-shell/assets/20579136/811d8fd9-a825-49db-b318-fbefc0db584d){align=right loading=lazy style="height:450px"}

    ![NWG Shell desktop screeshot](https://github.com/nwg-piotr/nwg-shell/assets/20579136/402bb806-ff9b-4869-aee0-683b7838e15d){align=right loading=lazy style="height:450px"}


    !!! EXAMPLE "Create USB Startup"
        ```shell
        dd bs=4M if=nwg-live-2024.10.01-x86_64.iso of=/dev/disk/by-id/usb-Generic_Flash_Disk_<id>-0:0 conv=fsync oflag=direct status=progress
        ```

    [Arch Linux ISO command line untilities](https://wiki.archlinux.org/title/USB_flash_installation_medium){target=_blank .md-button}

=== "Debian Linux"

    [Practicalli Journal - notes on Hyprland with Debian Linux](https://practical.li/journal/linux-wayland-compositor--hyprland/#hyprland-on-debian){target=_blank .md-button}

    !!! INFO "Hyprland on Debian - scripts to review "
        [hyprland on debian script](https://github.com/drewgrif/debian-hyprland){target=_blank .md-button}


    The hyprland packages are on version 0.41 at time of writing, the current release is 0.45 and has significant changes

=== "Arch Linux"

    Install a minimal Arch Linux OS from the ISO and setup Arch with Hyprland using the `archinstall`

    [Install the `nwg-shell` package](https://wiki.archlinux.org/title/Nwg-shell) to add tools for a Desktop Environment experience on top of Gnome.

    ```shell
    pacman -S nwg-shell
    ```

    Run the nwg-shell install script to configure the NWG tools
    ```shell
    nwg-shell-installer -w -hypr
    ```

    Set nwg-hello as the greeter (login screen) in `/etc/greetd/config.toml`

    ```config title="/etc/greetd/config.toml"
    [default_session]
    command = "Hyprland -c /etc/nwg-hello/hyprland.conf"
    ```

    [Arch Linux greetd.service](https://wiki.archlinux.org/title/Greetd){target=_blank .md-button}
