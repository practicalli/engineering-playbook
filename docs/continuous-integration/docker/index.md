# Docker

Docker enables a consistent approach to building and running Clojure projects along with a range of other services locally (database, cache, streams, etc.), The Clojure project is built from source when starting services (a `watch` feature can rebuild on code changes). Heath checks and conditions are set to ensure dependant services start in the correct order.

Running Docker is relatively fast once image overlays (layers) are cached on their first run, so its a viable approach for local system integration testing and acceptance testing, before pushing changes to a remote Continuous Integration service.

A Docker workflow complements a [REPL Driven Development workflow](https://practical.li/clojure/introduction/repl-workflow/), it does not replace it.  The main development effort should still be done via a REPL connected editor, with Docker Compose focused on orchestration of services.

## General Workflow

- Create a Clojure project, e.g. using [deps-new and Practicalli Project Templates](https://practical.li/clojure/clojure-cli/projects/templates/)
- Install Docker Desktop & Extensions, e.g. [Docker Desktop for Ubuntu](https://practical.li/blog/posts/docker-desktop-on-ubuntu-linux/)
- Create a Dockerfile ([multi-stage build and run-time](https://practical.li/blog/posts/build-and-run-clojure-with-multistage-dockerfile/))
- Compose services together, adding health checks and conditional starts
- REPL driven development, e.g. [Practicalli REPL Reloaded Workflow](https://practical.li/clojure/clojure-cli/repl-reloaded/)
- (optional) Automatic rebuild of Clojure project when [watching for code changes](https://docs.docker.com/compose/file-watch/) (experimental feature)

!!! HINT "Try the Docker getting started tutorial"
    Follow the Docker Getting Started tutorial from within Docker Desktop or via the command line.
    ```shell
    docker run -d -p 80:80 docker/getting-started
    ```

## Docker Desktop

[Docker desktop](https://docs.docker.com/desktop/) provides an easy way to manage Docker images, containers and volumes.  Sign in to Docker Desktop to manage your images on DockerHub.

![Docker Desktop - images view](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-desktop-images-light.png?raw=true)

There is a growing marketplace of extensions that provide very useful tools to extend the capabilities of Docker Desktop.  Search within the Docker Desktop extensions or for [extensions on Docker Hub](https://hub.docker.com/search?q=&type=extension).

- [Resource Usage](https://hub.docker.com/extensions/docker/resource-usage-extension) monitor resources (cpu, memory, network, disk) used by containers and docker compose systems over time
- [Disk Usage](https://hub.docker.com/extensions/docker/disk-usage-extension) optimise use of local disk space by removing unused images, containers and volumes
- [Volumes Backup & Share](https://hub.docker.com/extensions/docker/volumes-backup-extension) to backup, clone, restore and share Docker volumes easily
- [Logs Explorer](https://hub.docker.com/extensions/docker/logs-explorer-extension) view all container logs in one place to assist troubleshooting
- [Postgres Admin](https://hub.docker.com/extensions/mochoa/pgadmin4-docker-extension) PGAdmin4 open source management tool for Postgres
- [Trivy](https://hub.docker.com/extensions/aquasec/trivy-docker-extension) scan local and remote images for security vulnerabilities
- [Snyk](https://hub.docker.com/extensions/snyk/snyk-docker-desktop-extension) scan local and remote images for security vulnerabilities
- [Ddosify](https://hub.docker.com/extensions/ddosify/ddosify-docker-extension) high-performance, open-source and simple load testing tool

![Docker Desktop extension - disk usage](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-desktop-extensions-disk-usage-light.png?raw=true)

## Choosing Docker Images

[Docker Official Images](https://docs.docker.com/docker-hub/official_images/) from Docker Hub are highly recommended.  Look for the Docker Official Image tag on the image page.

![Docker Official Image Tag](https://docs.docker.com/docker-hub/images/official-image-badge-iso.png)

- [Clojure - official Docker Image](https://hub.docker.com/_/clojure/) - built by the Clojure community, provides tools to build Clojure projects (Clojure CLI, Leiningen)
- [Eclipse temurin OpenJDK - official Docker image](https://hub.docker.com/_/eclipse-temurin) - built by the [community](https://adoptium.net/) - provides the Java run-time
- [Amazon Corretto](https://hub.docker.com/_/amazoncorretto) is an OpenJDK distribution by Amazon AWS team, [Amazon Corretto](https://aws.amazon.com/corretto/) can also be installed for the local development environment
- [Postgres](https://hub.docker.com/_/postgres) open-source object-relational database management system
- [Redis](https://hub.docker.com/_/redis) open-source, networked, in-memory, key-value data store with optional durability
- [nginx](https://hub.docker.com/_/nginx) open source reverse proxy & load balancing for HTTP, HTTPS, SMTP, POP3 & IMAP protocols, HTTP cache and a web server
- [mariadb](https://hub.docker.com/_/mariadb) open source relational database by the original developers of MySQL and is much more efficient

??? INFO "Docker Official Image meaning"
    An Official Docker Image means the configuration of that image follows the [Docker recommended practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/), is well documented and designed for common use cases.

    There is no implication as to the correctness of tools, languages or service that image provides, only in the means in which they are provided.

    However, if time was invested in creating an image good enough to pass the Docker review, then it has a higher probability of being a useful image that others that are not official.

## Dockerfile Design

[:fontawesome-solid-book-open: Dockerfile design in detail](dockerfile.md){.md-button}

A multi-stage `Dockerfile` contains builder stage and an unnamed stage used as the run-time.  Optionally, the configuration can use a base image which both build and run-time stages extend.

The builder stage caches dependencies to optimise building Clojure and the run-time stage optimises running the service efficiently and securely.

The uberjar created by the builder image is copied over to the run-time image to keep that image as clean and small as possible (to minimise resource use).

![Docker Multi-stage build file with cached overlays](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-compose-build-output-cached-layers.png?raw=true)

- [Multi-stage `Dockerfile` for Clojure projects](https://practical.li/blog/posts/build-and-run-clojure-with-multistage-dockerfile/)
- [Docker Multi-stage builds docs](https://docs.docker.com/build/building/multi-stage/)

!!! WARNING "Docker init - beta feature"
    [`docker init`](https://docs.docker.com/engine/reference/commandline/init/) is a new (beta) feature to create `Dockerfile`, `.dockerignore` and`compose.yaml` files using Docker recommended practices.

## Compose services

Define a `compose.yaml` file that builds the Clojure project and run services that the Clojure service requires or talks too (database, cache, mock API, etc.).

Each service can define a heart beat which can be used as a conditional startup for other services.

??? INFO "compose.yaml new Compose configuration file"
    `compose.yaml` is the new configuration file for orchestrating services locally, a simplified and extended version of `docker-compose.yaml`.

Include the `build:` option for the Clojure service with the path to the multi-stage Dockerfile for the project (typically in the same root directory of the project, although a remote Git repository can also be used)

The Clojure service defines a dependency on a Postgres Database.  The dependency has a condition so the Clojure service is only started once the Postgres service is healthy

!!! EXAMPLE "Clojure Service with Postgres Database"
    ```yaml
    services:
      clojure-service:
        platform: linux/amd64
        build: ./
        ports: # host:container
          - 8080:8080
        depends_on:
          postgres-database:
            condition: service_healthy

      postgres-database:
        image: postgres:15.2-alpine
        environment:
          POSTGRES_PASSWORD: "$DOCKER_POSTGRES_ROOT_PASSWORD"
        healthcheck:
          test: [ "CMD", "pg_isready" ]
          timeout: 45s
          interval: 10s
          retries: 10
        ports:
          - 5432:5432
    ```

Run the services using docker from the root of the project

!!! NOTE ""
    ```shell
    docker compose up --build
    ```

### File watcher

Docker provides [`watch`](https://docs.docker.com/compose/file-watch/) as an experimental feature which can rebuild the Clojure service when a file change is detected.  This seems most useful when troubleshooting issues that occur during system integration testing.

Add an `x-develop` configuration with watch under the Clojure service configuration

!!! EXAMPLE "Automated rebuild on file change"
    ```yaml
        x-develop:
          watch:
            - path: ./deps.edn
              action: rebuild
            - path: ./src
              action: rebuild
    ```

Start the services and the file watch mode

!!! NOTE ""
    ```shell
    docker compose up --detach && docker compose alpha watch
    ```

Save changes to files and a new image for the Clojure service will be built and deployed when ready.


## Summary

Docker desktop provides lots of tools to support local system integration work before code is sent to a continuous integration service (or as a temporary alternative if that CI service id down)

Practicalli Project Templates include `Dockerfile`, `.dockerignore` and `compose.yaml` configurations for Clojure development, kick-starting the use of Docker.

Docker images are a relatively clean way of trying out different services or even different operating systems, e.g.[Ubuntu](https://hub.docker.com/_/ubuntu) or [ArchLinux](https://hub.docker.com/_/archlinux).  Deleting the images removes the whole service without affecting the underlying operating system.

[MegaLinter](https://practical.li/engineering-playbook/code-quality/megalinter/#megalinter-configuration) is an excellent example of a docker image that provides a large number of tools that would otherwise need to be installed directly on the operating system.

- [Docker Desktop Overview](https://docs.docker.com/desktop/)
- [Docker Desktop Extensions overview](https://docs.docker.com/desktop/extensions/)
- [Docker Build overview](https://docs.docker.com/build/)
- [Docker Compose Overview](https://docs.docker.com/compose/)
ocker images are a relatively clean way of trying out different services or even different operating systems, e.g.[Ubuntu](https://hub.docker.com/_/ubuntu) or [ArchLinux](https://hub.docker.com/_/archlinux)
