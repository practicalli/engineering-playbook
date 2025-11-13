# Debian Linux Install

Debian provides ISO images which can be burned onto  USB memory sticks (or other removable media, e.g. compact disks).

The net install image is small and quick to download, containing only the essential packages to run an operating system.

[Debian Linux NetInstall ISO image - AMD64](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-13.1.0-amd64-netinst.iso){.md-button}


## Create USB startup disk

Copy the Debian Linux ISO image to a USB of 1GB size or larger, using one of the following tools.

!!! NOTE "Create USB install disk using Caligula TUI"
    Install the [Caligula disk image TUI](https://practical.li/engineering-playbook/os/tui/#caligula).

    Run `calibula` with the burn command and the path to the downloaded Debian ISO image.  Follow the prompts of the Caligular TUI.

    ```shell
    Caligula burn debian.iso
    ```

!!! NOTE "Create USB install disk using the cp command"

    Find the name of the USB stick

    ```shell
    ls -l /dev/disk/by-id/usb-*
    ```

    Copy the ISO image to the USB disk (not a partition)

    ```shell
    cp debian.iso /dev/sda
    ```

??? NOTE "Creating USB install disks with dd"

    Copy the image using the name of the USB Stick
    ```shell
    dd bs=4M if=path/to/filename.iso of=/dev/disk/by-id/usb-My_flash_drive conv=fsync oflag=direct status=progress
    ```
    [Arch Linux: ISO image command line utilities](https://wiki.archlinux.org/title/USB_flash_installation_medium)
