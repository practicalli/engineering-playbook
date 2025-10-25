# Terminal User Interface (TUI)

Terminal or Text User Interface apps are very effective, especially for people who prefer the keyboard over mouse for control.

TUI's tend to use a lot less resources and therefore perform very well, regardless of operating system and available resources (CPU, Memory, Disk space, etc.).


??? INFO "NCurses - the original TUI library"
    [NCurses](https://invisible-island.net/ncurses/){target=_blank} is a programming library for creating textual user interfaces that work across a wide variety of terminals, written to optimize commands sent to the terminal to reduce latency when updating the displayed content.

    [NCurses - Wikipedia](https://en.wikipedia.org/wiki/Ncurses){target=_blank .md.button}


## Disk Management


### Caligula

[Caligula](https://github.com/ifd3f/caligula) is a TUI for imaging disks, often used to create a USB boot disk for Linux distributions.

A TUI alternative to the `dd` command.

Install options:

- GitHub release binaries for Linux and MacOSX (darwin)
- Arch Linux package `pacman -S caligula`
- Debian Linux has a [request to package](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1089012) discussion

Caligula is created with the [Rust Language](/engineering-playbook/programming-languages/rust/index.md) and can be installed via the `caligula` crate.

![Caligula copying an image to /dev/sdb](https://github.com/ifd3f/caligula/raw/main/images/verifying.png){align=left loading=lazy style="height:150px;width:150px"}


## OS Monitoring

### Bottom Btm

[bottom (btm)](https://github.com/ClementTsang/bottom){target=_blank} graphic process & system monitor.

=== "Debian Linux"

    ```shell
    apt install
    ```

=== "Arch Linux"

    ```shell
    sudo pacman -S btm
    ```

![bottom (btm) in action](https://github.com/ClementTsang/bottom/raw/main/assets/demo.gif){loading=lazy}


### ncdu disk usage

[ncdu](https://terminaltrove.com/ncdu/) is an ncurses disk usage analyzer.

=== "Debian Linux"

    ```shell
    apt install ncdu
    ```

=== "Arch Linux"

    ```shell
    sudo pacman -S ncdu
    ```

![ncdu disk usage analyzer](https://cdn.terminaltrove.com/m/a9ec3580-7859-4db7-be9f-ebd27578ab8d.png){loading=lazy}


## Version Control

### LazyGit

A Git client...


### Gitu

[Gitu](https://terminaltrove.com/gitu/){target=_blank} git client inspired by Magit.


![Gitu](https://cdn.terminaltrove.com/m/5bd52d18-974e-408c-8b00-88efe4e5d9bf.gif){loading=lazy}

=== "Arch Linux"

    ```shell
    sudo pacman -S gitu
    ```


## API tools

### fx json navigator

[fx] for viewing and effectively navigating JSON.  A TUI version of `jq`.

![Fx navigating JSON data](https://cdn.terminaltrove.com/m/34e03e9f-e7fc-4a99-8eb2-432d57291b25.gif){loading=lazy}


### jwt-ui

[jwt-ui](https://terminaltrove.com/jwt-ui/) Decoding and encoding JSON Web Tokens, for inspecting and debugging.

[:fontawesome-brands-github: GitHub releases](https://github.com/jwt-rs/jwt-ui/releases){target=_blank .md-button}

=== "Arch Linux"

    ```shell
    pacman -S jwt-ui
    ```

![jwt-ui](https://cdn.terminaltrove.com/m/a32f27a1-d1c9-4428-98b1-9e0fc3bb87a1.png){loading=lazy}


###

[](){target=_blank} git client inspired by Magit.

![](){loading=lazy}
