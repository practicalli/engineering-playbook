# Install via GitHub Release

Scripts can be used to automatically install software outside an operating system package management tools.

Compiled Binary releases and AppImages are commonly used to distribute software, building automatically via CI scripts and published as assets for a release on GitHub.

Scripts can be written to download these assets and place them in a suitable location in the execution path of the OS file system.

- Shared for all users: `/usr/local/bin`
- Only for current user: `$HOME/.local/bin`

> NOTE: An account with administrative privelleges (root or user with `sudo` group) is required to install in `/usr/local/bin`

---

### EXAMPLE "AppImage"

Download the AppImage binary for the latest release and save it in `$HOME/.local/bin/`

Script using `curl` to download the image and `sed` to transform text of the file name

```shell
curl -Lso ~/.local/bin/vifm \
    "https://github.com/vifm/vifm/releases/latest/download/vifm-v$(
        curl -Ls "https://api.github.com/repos/vifm/vifm/releases/latest" |
        sed -nE '/"tag_name":/s/.*"v*([^"]+)".*/\1/p'
    )-x86_64.AppImage" && chmod +x ~/.local/bin/vifm
```

Script using `wget` to download the AppImage and `jq` to manipulate the text of the file name.

```shell
wget -qO ~/.local/bin/vifm "$(
        wget -qO - "https://api.github.com/repos/vifm/vifm/releases/latest" |
        jq -r '.assets[] | select(.name|endswith(".AppImage")) | .browser_download_url'
    )" && chmod +x ~/.local/bin/vifm
```
