# Chosing a Linux Distribution


- Ubuntu
- Debian
- Arch Linux


## The essential differences


- package manager and package format
- default packages



## Ubuntu

Release schedule: 6 months + LTS 18 months

Uses the excellent `.deb` package format
Easy for those new to Linux as many choices are pre-made


Shifting to use snaps


> NOTE: why I am weary of snaps: multiple versions installed using up more space, self-contained app can duplicate libraries already on the system


## Debian Linux

Release schedule: Annually-ish + Rolling (testing)

Uses the excellent `.deb` package format

Easy for those new to Linux with a few extra choices to be made during install.


## Arch Linux

Release schedule: Rolling + Git

A good distribution if you want to learn how lots of things in Linux work.  Or if you want to create a very tailored experience.

There is good documentation, although it covers a wide range of options that can be overwhelming (even if you know what you want to do and what packages you want to use).

A significant time investment is required to learn all the important aspects of Arch Linux.

pacman is the default package manager.


There are several other package managers if packages from the Arch User? Repository (AUR) are wanted (TODO: why three and which is the most appropriate).  These package managers essentially create a package from source code and then install.


## Fedora / Centos

Uses the basic .rpm package format with very limited dependency management (a pale imitation of the `deb` format)


## Packaging methods


### Snaps

Packages are self-contained, so should work on a wider variety of distributions.

Sacrificing file space, memory use, startup time and efficient use of libraries (avoiding bloat) for the sake of convenience.


A well managed distribution that shares libraries effectively across packages is more efficient than


If a distribution did become mostly snaps then it would take over any other packaging manager.


> NOTE: I switched back to Debian when Ubuntu started only making key software available as snaps.
