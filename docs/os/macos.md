# Mac OS

A BSD based Unix system with commercial Graphic Desktop shell created by Apple.

    This section relies on commercial experiences when Practicalli was supplied with MacOSX and supporting hardware.

    Otherwise, testing relies on the community for confirmation and corrections.

!!! HINT "Use International English keyboard for sensible key locations"
    Practicalli highly recommends the 'International English' keyboard on Macbook laptops as this has sensible locations for keys, especially the `#` key.
    Or use the [:globe_with_meridians: Model 100 or Atreus keyboards from Keyboard.io](https://shop.keyboard.io/){target=_blank}

!!! INFO "Practicalli has limited access to MacOS"
    Practicalli only has occasional access to Mac hardware and macOS, typically during some commercial engagements.  Any updates or corrections to this section are most welcome

## Window Tiling manager

[Amethyst](https://ianyh.com/amethyst/){target=_blank} provides an automatic tiling window manager for MacOS.

Enable ++ctrl++ ++1++ ... ++5++ in the MacOS keybinding settings to control switching between the first 5 workspaces. Place the most commonly used apps on each of these workspaces to quickly switch between them.

[:fontawesome-brands-github: Amethyst Tiling window management - MacOSX](https://github.com/ianyh/Amethyst){target=_blank .md-button}

## Homebrew

Open Source development tools are available via homebrew, a community managed distribution of a wide range of tools.

!!! EXAMPLE "Install Homebrew"
    ```shell
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```

### Clojure

!!! INFO "OpenJDK required for Clojure"
    Install [OpenJDK from Adoptium](https://adoptium.net/) or search for `openjdk@21` on Homebrew

The Clojure maintainers provide a [Homebrew Tap for Clojure](https://github.com/clojure/homebrew-tools) which is recommeded over that provide by the Homebrew maintainers

!!! EXAMPLE "Install Clojure environment"
    ```shell
    brew install clojure/tools/clojure
    ```
> `bash` and `curl` are required, as is `rlwrap` if using the `clj` wrapper.
