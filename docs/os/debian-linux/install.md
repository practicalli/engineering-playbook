# Debian Linux Install

Debian provides ISO images which can be burned onto compact disks or USB memory sticks.

The net install image is small and quick to download, containing only the essential packages to run an operating system.

Copy the Debian Linux ISO image to a USB of 1GB size or larger

!!! EXAMPLE "Create USB install disk from Debian ISO file"
    Copy the ISO image to the USB disk (not a partition)
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
