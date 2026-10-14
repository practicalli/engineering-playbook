# Docker Desktop Overview

![Docker logo](https://github.com/practicalli/graphic-design/blob/live/topic-images/docker-logo-name.png?raw=true){align=right loading=lazy style="width:240px"}

[Docker desktop](https://docs.docker.com/desktop/) provides an easy way to manage Docker images, containers and volumes.  Sign in to Docker Desktop to manage your images on DockerHub.

![Docker Desktop - images view](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-desktop-images-light.png?raw=true#only-light)
![Docker Desktop - images view](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-desktop-images-dark.png?raw=true#only-dark)

[Docker Desktop Overview](https://docs.docker.com/desktop/){target=_blank .md-button}
[Docker Docs](https://docs.docker.com/){target=_blank .md-button}

## Extensions Marketplace

There is a growing marketplace of extensions that provide very useful tools to extend the capabilities of Docker Desktop.  Search within the Docker Desktop extensions or for [extensions on Docker Hub](https://hub.docker.com/search?q=&type=extension).

- [Resource Usage](https://hub.docker.com/extensions/docker/resource-usage-extension) monitor resources (cpu, memory, network, disk) used by containers and docker compose systems over time
- [Disk Usage](https://hub.docker.com/extensions/docker/disk-usage-extension) optimise use of local disk space by removing unused images, containers and volumes
- [Volumes Backup & Share](https://hub.docker.com/extensions/docker/volumes-backup-extension) to backup, clone, restore and share Docker volumes easily
- [Logs Explorer](https://hub.docker.com/extensions/docker/logs-explorer-extension) view all container logs in one place to assist troubleshooting
- [Postgres Admin](https://hub.docker.com/extensions/mochoa/pgadmin4-docker-extension) PGAdmin4 open source management tool for Postgres
- [Trivy](https://hub.docker.com/extensions/aquasec/trivy-docker-extension) scan local and remote images for security vulnerabilities
- [Snyk](https://hub.docker.com/extensions/snyk/snyk-docker-desktop-extension) scan local and remote images for security vulnerabilities
- [Ddosify](https://hub.docker.com/extensions/ddosify/ddosify-docker-extension) high-performance, open-source and simple load testing tool

![Docker Desktop extension - disk usage](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-desktop-extensions-disk-usage-light.png?raw=true#only-light)
![Docker Desktop extension - disk usage](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-desktop-extensions-disk-usage-dark.png?raw=true#only-dark)

[Docker Extensions Overview](extensions.md){target=_blank .md-button}

## Images

Pre-defined images can be used directly or used to build custom images.

[:fontawesome-solid-book-open: Docker Images Overview and Practicalli recommendations](../images.md){target=_blank .md-button}

## Containers

- container status
- view details
  - logs
  - inspect - environment, port values
  - terminal shell within the container
  - files - file browser for the container (check for uberjar, test-data, etc)
  - stats- basic resource usage  (Resource usage extension provides view over all containers)

- start / stop containers

## Volumes

Persist data to the local computer by mounting a part of the OS filespace as a volume in a Docker container.

## Dev Environments

<!-- TODO link to example dev environments - including one for Clojure - to submit  -->
