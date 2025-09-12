# Configure Hyprland

Hyprland comes with a simple config in `~/.config/hypr/hyprland.conf`.

Garuda Linux has a richer configuration, and an even more advanced config called G-Hyprland which includes some elements of nwg-shell.


## Monitors

Single monitor easy.

Multi-monitor

```shell
hyprctrl monitors
```

> NOTE: workspaces can be assigned to a specific monitor using [workspace rules](#workspace-rules).


## Status bar

Waybar


## Keyboard & Mouse


## Workspaces

A workspace is of of 10 numbered screen [1 to 0] within the hyprland virtual desktop.

++super+1++ switches focus to workspace number 1, replacing the current workspace if number 1 was not being displayed.

### Workspace rules

Define workspace rules which

- set name for each workspace
- assign workspace to specific monitor
- launch an app on the default workspaces
- set default workspace for each monitor


```conf
# Laptop monitor
workspace = 1, name:Kitty, monitor:DP-2, on-created-empty:kitty, default:true
workspace = 2, name:Neovim, monitor:DP-2
workspace = 3, name:Firefox, monitor:DP-2
workspace = 4, name:Data, monitor:DP-2
workspace = 5, name:Hacking, monitor:DP-2

# External Dell Monitor
workspace = 6, name:browser, monitor:eDP-1, on-created-empty:firefox, default:true
workspace = 7, name:Audio, monitor:eDP-1
workspace = 8, name:Graphics, monitor:eDP-1
workspace = 9, name:Screencast, monitor:eDP-1
workspace = 0, name:Config, monitor:eDP-1
```


## Layouts


## Wallpaper

wpaperd

hyprpaper


## File Manager

Thunar and Dolphin are highly themeable and work well with Hyprland.

=== "Thunar"

    !!! NOTE "Install Dolphin file manager"
        ```shell
        sudo pacman -S thunar
        ```

=== "Dolphin"

    !!! NOTE "Install Dolphin file manager"
        ```shell
        sudo pacman -S dolphin
        ```

    Dolphin theme may not show text correct

    !!! NOTE "KDE theme Configure"
        ```conf
        [UiSettings]
        ColorScheme=*
        ```

    !!! NOTE "Set KDE QPA Platform theme to qt6ct"
        ```conf title="~/.config/hypr/settings/manual_settings.conf"
        # Practicalli: add for dolphin file manager
        env = QT_QPA_PLATFORMTHEME,qt6ct
        ```

    Without KDE Plasma desktop, Dolphin doesn't have the default applications (or know about any applications) associated with each file type.

    KDE-applications rely on the KService desktop-file system configuration cache, [kbuildsycoca6](https://man.archlinux.org/man/kbuildsycoca6.8), for selecting desktop entries.

    The [Dolphin page on the Arch Linux Wiki](https://wiki.archlinux.org/title/Dolphin) suggests installing the `archlinux-xdg-menu` package

    Then run a command in a terminal to update the application menu:

    ```shell
    $ XDG_MENU_PREFIX=arch- kbuildsycoca6 --noincremental
    ```

    The `--noincremental` argument is optional.

    XDG_MENU_PREFIX is needed because `archlinux-xdg-menu` creates an XDG Desktop Menu with an arch- prefix (see [xdg-menu](https://wiki.archlinux.org/title/Xdg-menu)).

    The XDG Desktop Menu files can be found in `/etc/xdg/menus/*-applications.menu`.

    > NOTE: `kbuildsycoca6` is part of the kservice package, which is a dependency of dolphin, so normally shouldnt need to be installed manually.


!!! NOTE "Disable Application Not Responding dialog"
    Prevent the "Application Not Responding" dialog from showing, as its way too sensitive, especially copying files over the network.

    ```config title="~/.config/hypr/settings/manual_settings.conf"
    misc {
        enable_anr_dialog = false
    }
    ```



## Misc

Once your configuration is stable, consider disabling the automatic reload of configuration.  Use `hyprctrl reload` to manually update Hyprland.

```config
misc {
    disable_hyprland_logo = true

    # Practicalli
    # Application Not Responding dialog

    # Prevent file managers spamming with a dialog
    enable_anr_dialog = false

    # Increase missed 'alive' pings to limit how often the dialog shows
    # anr_missed_pings = 10

    # Disable automatic reload of the config when change detected
    # Use `hyprctrl reload` to update changes
    # Can save on power usage
    disable_autoreload = true
}
```

!!! INFO "Manually reload Hyprland configuration"
    ```shell
    hyprctrl reload
    ```

### Wallpaper

The wpaperd GitHub readme has a detailed section on [wallpaper configuration](https://github.com/danyspin97/wpaperd?tab=readme-ov-file#wallpaper-configuration){target=_blank}

The [duration time format](https://docs.rs/humantime/latest/humantime/fn.parse_duration.html){target=_blank} is quite flexible.  I set the wallpaper to change once a day, every 24 hours.

A wallpaper can be set for a specific monitor or use `any` as a fall-back wallpaper.

`hyprctrl monitors` command shows the names used to refer to each monitor detected.

> NOTE: To use a fixed wallpaper, specify the filename of the image in the path rather than a directory.

!!! EXAMPLE "Configure Wpaperd for multiple monitors"
    ```toml title=".config/wpaperd/config.toml"
    [default]
    transition-time = 600
    duration = '24 hours'

    [any]
    path = "~/Pictures/wallpaper/hyprland/"

    # Laptop monitor
    [eDP-1]
    path = "~/Pictures/wallpaper/favorites/"

    # External monitor display link
    [DP-2]
    path = "~/Pictures/wallpaper/practicalli/"
    ```

!!! INFO "Wpaperd image format support"
    wpaperd support image formats: AVIF, BMP, DDS, EXR, FF, GIF, HDR, ICO, JPEG, PNG, PNM, QOI, TGA, TIFF, and WebP

    [:fontawesome-brands-github: image-rs/image readme](https://github.com/image-rs/image/blob/main/README.md#supported-image-formats){target=_blank}
