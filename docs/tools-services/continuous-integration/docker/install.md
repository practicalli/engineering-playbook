# Install Docker Desktop

![Docker logo](https://github.com/practicalli/graphic-design/blob/live/topic-images/docker-logo-name.png?raw=true){align=right loading=lazy style="width:240px"}

Docker community edition provides the back-end services to run docker images in containers.

Docker Desktop provides a graphical UI for managing images, containers and volumes.

## Install Docker Desktop

Docker desktop depends on Docker Community Edition and will install all the respective packages.

=== "Debian Linux"
    [Install Docker Desktop on Debian - Docker Docs](https://docs.docker.com/desktop/setup/install/linux/debian/){target=_blank .md-button}

    Debian Linux based distributions has several prerequisites packages (may already be installed).

    ```shell
    apt install ca-certificates curl
    ```

    Add the Docker team public key to Debian package manager, ensuring only official Docker packages are used

    ```shell
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc && \
    chmod a+r /etc/apt/keyrings/docker.asc
    ```

    Add the Docker PPA to the Debian package manager, creating a `/etc/apt/sources.list.d/docker.list` file.

    ```shell
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

    Update the available packages list with the Docker repository
    ```shell
    apt-get update
    ```

    [:globe_with_meridians: Download the DEB package](https://docs.docker.com/desktop/install/ubuntu/) for Docker Desktop UI and install using Debian package manager
    ```shell
    apt install ./Downloads/docker-desktop-4.19.0-amd64.deb
    ```

### Post Install

User accounts that will run docker should be included in the `docker` group.

=== "Debian Linux"
    Add the current operating system user account to the `docker` operating system group, creating the `docker` group if it doesn't already exist

    ```
    groupadd docker && \
    usermod -aG docker $USER
    ```

   Logout of the current session (e.g. logout of Desktop) to apply the `docker` group to the user account.

   `groups` command will list the operating system groups assigned to the current user.

## Start Docker & Docker Desktop

Starting Docker Desktop will automatically start the underlying Docker community edition that provides the run-time for docker containers.

Launch Docker Desktop via the desktop application Launcher or via the command line:

=== "Debian Linux"
    Use the Debian application launcher to start Docker Desktop, or use the `systemctl` command from a terminal.

    ```shell
    systemctl --user start docker-desktop
    ```

> Docker desktop may automatically restart itself on first run

## Check Docker works

Use the [Docker tutorial](https://www.docker.com/101-tutorial/) image to check that Docker can run a container from an image (and also learn about Docker if new to the tools).

```shell
docker run -dp 80:80 docker/getting-started
```

The tutorial image will be downloaded and the image run in a container

```shell
❯ docker run -dp 80:80 docker/getting-started

Unable to find image 'docker/getting-started:latest' locally
latest: Pulling from docker/getting-started
c158987b0551: Already exists
1e35f6679fab: Pull complete
cb9626c74200: Pull complete
b6334b6ace34: Pull complete
f1d1c9928c82: Pull complete
9b6f639ec6ea: Pull complete
ee68d3549ec8: Pull complete
33e0cbbb4673: Pull complete
4f7e34c2de10: Pull complete
Digest: sha256:d79336f4812b6547a53e735480dde67f8f8f7071b414fbd9297609ffb989abc1
Status: Downloaded newer image for docker/getting-started:latest
215c033924260874013394d1f27fa5ec587f183ee9851d3a48884a1422fcc732
```

Open the tutorial website at [http://localhost/](http://localhost/) and follow the tutorial steps to learn more about Docker.

!!! HINT "Increase concurrent download of image overlays"
    increase the max-concurrent-downloads value from the default 3 to 24, to download multi-layered docker images faster
    ```config hl_lines="5" title=".docker/config.json"
    {
     "auths": {},
     "credsStore": "desktop",
     "currentContext": "default",
     "max-concurrent-downloads": 24
    }
    ```

## Check installed versions

Print the version of Docker installed and if Docker Desktop is running, then its version is also printed.

!!! NOTE ""
    ```shell
    docker version
    ```

Example
??? EXAMPLE "Output from running Docker Desktop"
    ```shell
    ❯ docker version
    Client: Docker Engine - Community
     Cloud integration: v1.0.31
     Version:           23.0.6
     API version:       1.42
     Go version:        go1.19.9
     Git commit:        ef23cbc
     Built:             Fri May  5 21:18:13 2023
     OS/Arch:           linux/amd64
     Context:           desktop-linux

    Server: Docker Desktop 4.19.0 (106363)
     Engine:
      Version:          23.0.5
      API version:      1.42 (minimum version 1.12)
      Go version:       go1.19.8
      Git commit:       94d3ad6
      Built:            Wed Apr 26 16:17:45 2023
      OS/Arch:          linux/amd64
      Experimental:     false
     containerd:
      Version:          1.6.20
      GitCommit:        2806fc1057397dbaeefbea0e4e17bddfbd388f38
     runc:
      Version:          1.1.5
      GitCommit:        v1.1.5-0-gf19387a
     docker-init:
      Version:          0.19.0
      GitCommit:        de40ad0
    ```

!!! NOTE "Check compose version"
    ```shell
    docker compose version
    ```

??? Example "Example output"
    ```shell
    ❯ docker compose version
    Docker Compose version v2.17.3
    ```

## Optimise Log rotation

Docker uses the `json-file` driver which creates JSON objects of log events from all containers.  To avoid disk space issues, configure log rotation in a `/etc/docker/daemon.json` file

!!! INFO "Configure Log rotation"
    ```json title="/etc/docker/daemon.json"
    {
      "log-driver": "json-file",
      "log-opts": {
        "max-size": "10m",
        "max-file": "3"
      }
    }
    ```

Use the [local file logging driver](https://docs.docker.com/config/containers/logging/local/) if a longer logging history is desirable.  The local file logging driver preserves 100Mb of logs per container (5 x 20Mb files) and uses automatic compression to greatly reduce disk consumption.

!!! INFO "Use local file logging driver"
    ```json title="/etc/docker/daemon.json"
    {
      "log-driver": "local",
      "log-opts": {
        "max-size": "10m"
      }
    }
    ```

<!-- TODO: does local logging driver work with Docker Desktop log explorer? -->

> --log-driver flag with `docker container create` or `docker run` commands sets the log driver for the specific container, over-riding the global setting

* [JSON File logging driver](https://docs.docker.com/config/containers/logging/json-file/)
