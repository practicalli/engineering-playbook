# Install Docker CE and Desktop

Docker community edition provides the back-end services to run docker images in containers.

Docker Desktop provides a graphical UI for managing images, containers and volumes.


## Install Docker Community Edition

=== "Ubuntu / Debian Install"

    Install via the package archive manage by the Docker team.  Add the Docker team public key and archive, then install the Docker community edition (CE) related packages.

    Ubuntu Prerequisites to use the Docker team keys (may already be installed).

    ```shell
    sudo apt-get install ca-certificates curl gnupg lsb-release
    ```

    Add the Docker team public key to Ubuntu package manager, ensuring only official Docker packages are used

    ```shell
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    ```

    Add the Docker PPA to the Ubuntu package manager, creating a `/etc/apt/sources.list.d/docker.list` file.

    ```shell
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

    Install Docker community edition packages

    ```shell
    sudo apt update && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-desktop
    ```

## Install Docker Desktop


=== "Ubuntu / Debian Install"
    [:globe_with_meridians: Download the DEB package](https://docs.docker.com/desktop/install/ubuntu/) for Docker Desktop UI and install using Ubuntu package manager
    ```shell
    sudo apt install ./Downloads/docker-desktop-4.19.0-amd64.deb
    ```

### Post Install

Docker runs under the `docker` operating system group for greater security.  User accounts should be included in the `docker` group to run Docker community edition.

=== "Ubuntu / Debian Install"
    Add the current operating system user account to the `docker` operating system group, creating the `docker` group if it doesn't already exist

    ```
    sudo groupadd docker
    sudo usermod -aG docker $USER
    ```

    A user must completely logout of the current login session before the `docker` group is applied.

    `groups` lists all the operating system groups the current user is assigned to.


## Start Docker & Docker Desktop

Starting Docker Desktop will automatically start the underlying Docker community edition that provides the run-time for docker comtainers.

=== "Ubuntu / Debian Install"
    Use the Ubuntu application launcher to start Docker Desktop, or use the `systemctl` command from a terminal.

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

Print the version of Docker CE installed.  If Docker Desktop is running, then version its information is also printed.

```shell
docker version
```

Example output (once Docker Desktop is running)

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

Check compose version

```shell
docker compose version
```

Example output

```shell
❯ docker compose version
Docker Compose version v2.17.3
```


## Optimise Log rotation

Docker uses the `json-file` driver which creates JSON objects of log events from all containers.  To avoid disk space issues, configure log rotation in a `/etc/docker/daemon.json` file

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

Use the [local file logging driver](https://docs.docker.com/config/containers/logging/local/) if a longer logging history is desirable.  The local file logging driver preserves 100Mb of logs per container (5 x 20Mb files) and uses automatic compression to greatly reduce disk consumption.

```json
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
