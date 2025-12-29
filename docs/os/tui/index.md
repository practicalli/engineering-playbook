# Terminal User Interface (TUI)

Terminal or Text User Interface apps are very effective, especially for people who prefer the keyboard over mouse for control.

TUI's tend to use a lot less resources and therefore perform very well, regardless of operating system and available resources (CPU, Memory, Disk space, etc.).


??? INFO "NCurses - the original TUI library"
    [NCurses](https://invisible-island.net/ncurses/){target=_blank} is a programming library for creating textual user interfaces that work across a wide variety of terminals, written to optimize commands sent to the terminal to reduce latency when updating the displayed content.

    [NCurses - Wikipedia](https://en.wikipedia.org/wiki/Ncurses){target=_blank .md.button}


## Disk Management

Viewing disk usage and coppying disk images to removable medial (e.g. USB memory sticks).

### Caligula

[Caligula](https://github.com/ifd3f/caligula) is a TUI for imaging disks, often used to create a USB boot disk for Linux distributions.

A TUI alternative to the `dd` command.

Install options:

- GitHub release binaries for Linux and MacOSX (darwin)
- Arch Linux package `pacman -S caligula`
- Debian Linux has a [request to package](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1089012) discussion

Caligula is created with the [Rust Language](/engineering-playbook/programming-languages/rust/index.md) and can be installed via the `caligula` crate.

![Caligula copying an image to /dev/sdb](https://github.com/ifd3f/caligula/raw/main/images/verifying.png){loading=lazy}

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


### diskonaut


[diskonaut disk usage explorer](https://github.com/imsnif/diskonaut){target=_blank} is a visual treemap representation of disk space usage. Delete files or directories and diskonaut will track how much space you've freed up in this session.

![diskonaut disk usage explorer](https://github.com/imsnif/diskonaut/raw/main/demo.gif){loading=lazy}


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


### envx environment variable manager

[envx](https://github.com/mikeleppane/envx){target=_blank} environment variable manager.

![Environment Variable Manager explore variables](https://github.com/mikeleppane/envx/raw/main/images/main.png){loading=lazy}


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

[jwt-ui](https://github.com/jwt-rs/jwt-ui) Decoding and encoding JSON Web Tokens, for inspecting and debugging.

[:fontawesome-brands-github: GitHub releases](https://github.com/jwt-rs/jwt-ui/releases){target=_blank .md-button}

=== "Arch Linux"

    ```shell
    pacman -S jwt-ui
    ```

![jwt-ui](https://github.com/jwt-rs/jwt-ui/raw/main/screenshots/decoder.png){loading=lazy}


## Tracking Work

### GitHub CLI


### Jirust

[Jirust](https://github.com/Code-Militia/jirust){target=_blank} Jira in the terminal.

![Jirust in action](https://user-images.githubusercontent.com/7011993/225179809-b4683ea5-93e5-4c4c-abf5-e6534df0f5a3.gif){loading=lazy}


### Lt - Linear client

[lt](https://github.com/markmarkoh/lt){target=_blank} view issues from [Linear.app](https://linear.app/) issue tracker & project management tool.

> NOTE: read only client initially

![lt showing issues and details](https://github.com/user-attachments/assets/dd29d164-4ec8-4bb6-b469-667680b2d739){loading=lazy}


### isw - pomodoro timer

https://gitlab.com/thom-cameron/isw

https://gitlab.com/thom-cameron/isw/-/raw/main/repo_assets/screenshot.png


## File management

### nnn

[nnn](https://github.com/jarun/nnn){target=_blank}

Install via OS package manager (Debian, Arch, etc)

=== "Debian Linux"
    ```shell
    apt install nnn mediainfo libimage-exiftool-perl atool patool vlock lftp sshfs
    ```

    > NOTE: `libimage-exiftool-perl` is installed instead of 'exiftool'

[nnn.nvim plugin](https://github.com/luukvbaal/nnn.nvim){target=_blank}


### Vifm Vim-like file manager

[Vifm](https://vifm.info/){target=_blank} is a curses based Vim-like file manager extended with some useful ideas from mutt. Vifm uses common vim-style key bindings to navigate and manipulate files and directories.

Vifm is [packaged for Linux/Unix based operating systems](https://github.com/vifm/vifm?tab=readme-ov-file#installation). An [AppImage is available](https://github.com/vifm/vifm?tab=readme-ov-file#appimage) if an OS package is not available.

=== "Debian Linux"
    ```shell
    apt install vifm
    ```


### igrep interactive grep

[igrep](https://github.com/konradsz/igrep){target=_blank}

![igrep](https://github.com/konradsz/igrep/raw/main/assets/v1_0_0.gif){loading=lazy}


## Hardware

### kbt keyboard tester

[kbt](https://github.com/bloznelis/kbt){target=_blank} is a keyboard tester that supports multiple keyboard layouts.

> NOTE: wayland support pending


### Bluetui

[Bluetui](https://github.com/pythops/bluetui){target=_blank} for managing bluetooth on Linux.

![bluetui showing bluetooth devices](https://github.com/user-attachments/assets/f937535d-5675-4427-b347-8086c8830e23){loading=lazy}


### Blendr bluetooth analyser

[blendr](https://github.com/dmtrKovalenko/blendr){target=_blank} to inspect, search, connect, and analyze data coming from BLE devices directly from your terminal.

Designed for BLE engineers to useful search, create a direct connection to any characteristic and device with one command, and displaying your custom services names in the UI.

![blendr screen shot](https://github.com/dmtrKovalenko/blendr/raw/main/demo.png){loading=lazy}


## Website readers

### Feedr

[Feedr](){target=_blank} to manage and read RSS feeds with elegant visuals and smooth keyboard navigation.

Install from GitHub release or via Rust Cargo.

![](https://github.com/bahdotsh/feedr/raw/main/assets/images/feedr.png){loading=lazy}


### bbcli BBC News reader

[bbcli](https://github.com/hako/bbcli){target=_blank} news reader from BBC.co.uk with vim-style navigation.

`--feed` argument to show a [specific news topic](https://github.com/hako/bbcli#available-feeds).

```shell
# List top stories
bbcli list

# List from specific feed
bbcli --feed world list
bbcli --feed technology list
bbcli --feed business list
```

![bbcli showing news from bbc.co.uk](https://raw.githubusercontent.com/hako/bbcli/refs/heads/master/bbcli.gif){loading=lazy}


## To Categories

### fzf-make

[fzf-make](https://github.com/kyu08/fzf-make){target=_blank} Tui for make, just, pnpm, yarm, task.  Search and browse task definitions, keeping a history of tasks used.

Install via Rust Cargo, Arch Linux, Nix, Homebrew

![fzf-make in action](https://raw.githubusercontent.com/kyu08/fzf-make/main/static/demo.gif){loading=lazy}


### csvlens

[csvlens](https://github.com/YS-L/csvlens){target=_blank} explorer for Comma Seperated Values (CSV data).  Supports basic vim-style navigation.

![cvslens exploring a CSV file](https://github.com/YS-L/csvlens/raw/main/.github/demo.gif){loading=lazy}


### xan

[xam](https://github.com/medialab/xan){target=_blank} process CSV files from the shell, previewing data as graphs.


![xam facet grid example](https://github.com/medialab/xan/raw/master/docs/img/grid/small-multiples.png){loading=lazy}


### flawz

[flawz](https://github.com/orhun/flawz){target=_blank} to browse security vulnerabilities (CVEs) in the NVD database from NIST.

Install from GitHub releases, Arch Linux, Homebrew, Nix, NetBSD.

![flawz browsing security vulnerabilities](https://github.com/orhun/flawz/raw/main/assets/demo.gif){loading=lazy}


### GPGTui

GPGTui for GPG interactive key management (listing/exporting/signing) with command-line fallback for complex operations.

![GPGTui](https://github.com/orhun/gpg-tui/raw/master/demo/gpg-tui-help_menu.gif){loading=lazy}


### md-tui

[md-tui](https://github.com/henriklovhaug/md-tui){target=_blank} for viewing markdown files.

<!-- ![](){loading=lazy} -->


### mprocs

[mprocs](https://github.com/pvolok/mprocs){target=_blank}

<!-- ![](){loading=lazy} -->



### iamb matrix client

[iamb](https://github.com/ulyssa/iamb){target=_blank} Matrix client for the terminal that uses Vim keybindings.

![iamb in action](https://camo.githubusercontent.com/c725d5bd68a5f7f6e14c9221234ff76c40888c9dc87b9fe85431cf07faa40928/68747470733a2f2f69616d622e636861742f7374617469632f696d616765732f69616d622d64656d6f2e676966){loading=lazy}

### oxker docker containers

[oxker](https://github.com/mrjackwills/oxker){target=_blank} view and control docker containers.

![oxker in action ](https://github.com/mrjackwills/oxker/raw/main/.github/demo_01.webp){loading=lazy}


### ttyper

Test and enhance your typing skills. [Typer](https://github.com/max-niederman/ttyper){target=_blank} provides a detailed report of your typing skills.

![Typer typing test and reports](https://github.com/max-niederman/ttyper/raw/main/resources/recording.gif){loading=lazy}


### TTYPR

[TTYPR](https://github.com/hotellogical05/ttypr){target=_blank} is a simple, lightweight typing practice application.

![TTYPR in action](https://github.com/hotellogical05/ttypr/raw/main/images/preview.gif){loading=lazy}

Installed via Rust Cargo package manager.



[](){target=_blank}

![](){loading=lazy}
