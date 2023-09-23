# Debian Linux

Debian Linux provides support for the widest range of hardware of any Linux distribution.

The Debian project established the high-quality `.deb` package system which ensures all packages are consitenly defined and manage dependencies. 


## Install

Debian provides ISO images which can be burned onto compact disks or USB memory sticks.

The net install image is small and quick to download, containing only the essential packages to run an operating system.


## Post Install



### root vs sudo

Debian recommends using the root account to administer the system, rather than using `sudo` as with Ubuntu 

`su -` command in a terminal changes to the root user, upon entering a successful root password at the prompt

```shell
su -
```

> root password is set during install of Debian


!!! HINT "Dedicated terminal for root account"
    Open a terminal application specifically to use the root account when carrying out significant maintenance, e.g. installing many packages during the post install.


## Set XDG freedesktop locations



## Git install

root@londo:~# apt install git
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  git-man liberror-perl patch
Suggested packages:
  git-daemon-run | git-daemon-sysvinit git-doc git-email git-gui gitk
  gitweb git-cvs git-mediawiki git-svn ed diffutils-doc
The following NEW packages will be installed:
  git git-man liberror-perl patch
0 upgraded, 4 newly installed, 0 to remove and 0 not upgraded.
Need to get 9,377 kB of archives.
After this operation, 48.0 MB of additional disk space will be used.
Do you want to continue? [Y/n]


## Clone practicalli/dotfiles

clone to projects/practicalli/dotfiles


## Git configure

link .config/git to Practicall dotfiles/git directory

Update identity-practicalli-john file with annonymous email address from GitHub settings > Emails section



## Kitty Terminal

Debian packages 

- kitty
- kitty-doc
- kitty-shell-integration  
- kitty-terminfo           
- `fonts-firacode` use by Practicalli Kitty terminal


```shell
apt install kitty kitty-doc kitty-shell-integration kitty-terminfo fonts-firacode

```
root@londo:~# sudo apt install kitty kitty-doc kitty-shell-integration kitty-terminfo 
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libb2-1 libjs-jquery libjs-sphinxdoc libjs-underscore librsync2
Suggested packages:
  imagemagick
The following NEW packages will be installed:
  kitty kitty-doc kitty-shell-integration kitty-terminfo libb2-1 libjs-jquery
  libjs-sphinxdoc libjs-underscore librsync2
0 upgraded, 9 newly installed, 0 to remove and 0 not upgraded.
Need to get 4,106 kB of archives.
After this operation, 20.6 MB of additional disk space will be used.
Do you want to continue? [Y/n] 


Link to practicalli/dotfiles/kitty in .config directory


## Zsh and Prezto

Install zsh package using su -

as the user account, `zsh` command to change to zsh from bash shell

check if XDG_CONFIG_HOME is set, if not set to $HOME/.config on command line (practicalli dotfiles has a zshenv that does this later in process)

clone prezto to XDG location

link customised configuration files to practicalli dotfiles/zsh 

- .zprezto practicalli customisations for Prezto
- .zprofile includes .local/bin on execution path
- .zshrc aliases for neovim configurations (astro, practicalli
- .zshenv configures XDG locations 

> cd .config/zsh
> ls -la
total 112
drwxr-xr-x  3 practicalli practicalli  4096 Aug  9 16:10 .
drwx------ 12 practicalli practicalli  4096 Aug  8 20:54 ..
drwxr-xr-x  6 practicalli practicalli  4096 Aug  8 19:09 .zprezto
-rw-r--r--  1 practicalli practicalli 92846 Aug  8 20:45 .p10k.zsh
lrwxrwxrwx  1 practicalli practicalli    53 Aug  8 20:35 .zlogin -> /home/practicalli/.config/zsh/.zprezto/runcoms/zlogin
lrwxrwxrwx  1 practicalli practicalli    54 Aug  8 20:35 .zlogout -> /home/practicalli/.config/zsh/.zprezto/runcoms/zlogout
lrwxrwxrwx  1 practicalli practicalli    62 Aug  8 20:38 .zpreztorc -> /home/practicalli/projects/practicalli/dotfiles/zsh/.zpreztorc
lrwxrwxrwx  1 practicalli practicalli    55 Aug  8 20:35 .zprofile -> /home/practicalli/.config/zsh/.zprezto/runcoms/zprofile
lrwxrwxrwx  1 practicalli practicalli    59 Aug  8 20:36 .zshenv -> /home/practicalli/projects/practicalli/dotfiles/zsh/.zshenv
-rw-------  1 practicalli practicalli  1460 Aug  9 16:10 .zsh_history
lrwxrwxrwx  1 practicalli practicalli    58 Aug  8 20:36 .zshrc -> /home/practicalli/projects/practicalli/dotfiles/zsh/.zshrc



## Neovim 

Required packages

- `curl` - suggested by neovim checkhealth
- `gdu`
- `xclip` provider for accessing operating system clipboard from Neovim
- `luarocks` required for some mason installed tools
- `lazygit` terminal UI git client used by AstroNvim


```shell
apt install curl gdu lazygit luarocks xclip
```

> `btm` is suggested but not available as a Debian packages

### Nodejs

Nodejs is required for many of the linters installed by Mason.

`node` and `npm` are available as packages, although the `npm` package has a great many dependencies (many of which seem redundant).

Download from nodejs website

Extract to `~/.local/apps/nodejs`

Create a symbolic link to make it simpler to use alternate versions of node and npm

```shell
ln -s ~/.local/apps/nodejs/node-v18.17.1-linux-x64/ ~/.local/apps/nodejs/current
```

Create symbolic links for `node` and `npm` to add them to the OS excecution path

```shell
ln -s ~/.local/apps/nodejs/current/bin/node ~/.local/bin/node &&
ln -s ~/.local/apps/nodejs/current/bin/npm ~/.local/bin/npm &&
```

If a different version of nodejs is installed, then all that should be required is deleting and recreating the `current` symbolic link in `~/.local/apps/nodejs/` to point to the desired version.


### Releases

Download latest appimage for neovim from Neovim repository releases page

chmod u+x nvim.appimage

> Debian specific? - mkdir ~/.local/bin

mv nvim.appimage ~/.local/bin

ln -s ~/.local/bin/nvim.appimage ~/.local/bin/nvim


Edit .config/zsh/.zprofile and add $HOME/.local/bin to paths to search for executables

Set the list of directories that Zsh searches for programs.
path=(
  $HOME/.local/{,s}bin(N)
  /opt/{homebrew,local}/{,s}bin(N)
  /usr/local/{,s}bin(N)
  $path
)

source .zprofile to  update the path and include the .local/bin

```shell
source .config/zsh/.zprofile 
```


### Configure nvim

Clone astronvim configuration and Practicalli Astronvim configuration

Add aliases to .z call different neovim configurations

alias astro="NVIM_APPNAME=astronvim nvim"
alias lazyvim="NVIM_APPNAME=lazyvim nvim"
alias practicalli-redux="NVIM_APPNAME=neovim-config-redux nvim"

