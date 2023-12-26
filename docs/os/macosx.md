# Mac OSX

A BSD based Unix system with commercial Graphic Desktop shell created by Apple.

!!! WARNING "Practicalli does not own Mac hardware config untested"
    This section relies on commercial experiences when Practicalli was supplied with MacOSX and supporting hardware.

    Otherwise, testing relies on the community for confirmation and corrections.


## Yabai Tiling manager

[Yabai Tiling window management - MacOSX](https://github.com/koekeishiya/yabai){target=_blank .md-button} 


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

