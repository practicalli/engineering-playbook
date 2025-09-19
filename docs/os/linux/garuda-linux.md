# Garuda Linux

A professionally assembled operating system based on Arch Linux, with variants including Hyprland.


## Thunar file manager

Thunar is the default file manager in Garuda Linux Hyprland.

Thunar can be used to access shared filespace across the network using SFTP.

```shell
sftp://practicalli@rangerone/home/practicalli
```

A prompt appears to enter the user's password.


### Window rules 

Thunar dialogs show as a full size tiled window, taking up a disproportionate amount of space.

A cleaner desktop is achived by setting window rules to show dialogs as a floating windows.  Optionally moving the windows to the mouse cursor position or to a suitable position on the current screen.

The Rename dialog should keep focus even if the mouse is not hovering over the dialog.  The focus will change if the mouse is moved to another window though.

Center the Rename dialog on the mouse will minimise focus being lost if the mouse is moved slightly.


!!! NOTE "Thunar Rename dialog: Center on mouse cursor"
    ```Configure
    windowrule = float,stayfocused,move cursor -50% -50%,class:^(thunar)$,title:(Rename.*)$
    ```

The File Operations Progress 

!!! NOTE "Thunar File Operations: float and move to bottom left corner"
    ```Configure
    windowrule = float,class:^(thunar)$,title:(File Operation Progress.*)$
    windowrule = move onscreen 100%-24 100%-24,class:^(thunar)$,title:(File Operation Progress.*)$
    ```
```

