# Hyprland Tiling Compositor


Hyprland is an elegant and high performance Tiling window compositor for Wayland on Linux.

Hyprland is not a full desktop environment, unlike KDE or Gnome.  Additional tools can be installed to make a complete desktop environment.

!!! HINT "Try Hyprland with the NWG Live ISO"
    [NWG Live ISO](https://github.com/nwg-piotr/nwg-iso) provides a simple way to try Hyprland and the set of tools provided by NWG-Shell to give a Desktop Environment experience based on Gnome.


## Install

!!! WARNING "Hyprland is a quickly evolving project and issues may occur"

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
