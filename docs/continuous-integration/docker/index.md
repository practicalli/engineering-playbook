# Docker

Docker enables a consistent approach to building and running Clojure projects along with a range of other services locally (database, cache, streams, etc.), The Clojure project is built from source when starting services (a `watch` feature can rebuild on code changes). Heath checks and conditions are set to ensure dependant services start in the correct order.

Running Docker is relatively fast once image overlays (layers) are cached on their first run, so its a viable approach for local system integration testing and acceptance testing, before pushing changes to a remote Continuous Integration service.

A Docker workflow complements a [REPL Driven Development workflow](https://practical.li/clojure/introduction/repl-workflow/), it does not replace it.  The main development effort should still be done via a REPL connected editor, with Docker Compose focused on orchestration of services.

## General Workflow

- Create a Clojure project, e.g. using [:globe_with_meridians: deps-new and Practicalli Project Templates](https://practical.li/clojure/clojure-cli/projects/templates/){target=_blank}
- [:fontawesome-solid-book-open: Install Docker Desktop](install.md) & [:fontawesome-solid-book-open: Extensions](desktop-extensions.md)
- [:fontawesome-solid-book-open: Select a Docker image](images.md) as a base or builder stage
- [:fontawesome-solid-book-open: Create a Dockerfile](dockerfile.md), e.g [:fontawesome-solid-book-open: a multi-stage build and run-time Dockerfile](clojure-multi-stage-dockerfile.md)
- [:fontawesome-solid-book-open: Compose services together](compose.md), adding health checks and conditional starts
- REPL driven development, e.g. [:globe_with_meridians: Practicalli REPL Reloaded Workflow](https://practical.li/clojure/clojure-cli/repl-reloaded/)
- (optional) Automatic rebuild of Clojure project when [:fontawesome-solid-book-open: watching for code changes](compose.md#build-on-change) (experimental feature)

!!! HINT "Try the Docker getting started tutorial"
    Follow the Docker Getting Started tutorial from within Docker Desktop or via the command line.
    ```shell
    docker run -d -p 80:80 docker/getting-started
    ```

## Choosing Docker Images

![Docker Official Image Tag](https://docs.docker.com/trusted-content/images/official-image-badge-iso.png){align=right}

[Docker Official Images](https://docs.docker.com/docker-hub/official_images/) are highly recommended.  Look for the Docker Official Image tag on the image page.

??? INFO "Docker Official Image meaning"
    An Official Docker Image means the configuration of that image follows the [Docker recommended practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/), is well documented and designed for common use cases.

    There is no implication as to the correctness of tools, languages or service that image provides, only in the means in which they are provided.

    However, if time was invested in creating an image good enough to pass the Docker review, then it has a higher probability of being a useful image that others that are not official.

- [Clojure - official Docker Image](https://hub.docker.com/_/clojure/) - built by the Clojure community, provides tools to build Clojure projects (Clojure CLI, Leiningen)
- [Eclipse temurin OpenJDK - official Docker image](https://hub.docker.com/_/eclipse-temurin) - built by the [community](https://adoptium.net/) - provides the Java run-time
- [Amazon Corretto](https://hub.docker.com/_/amazoncorretto) is an OpenJDK distribution by Amazon AWS team, [Amazon Corretto](https://aws.amazon.com/corretto/) can also be installed for the local development environment
- [Postgres](https://hub.docker.com/_/postgres) open-source object-relational database management system
- [Redis](https://hub.docker.com/_/redis) open-source, networked, in-memory, key-value data store with optional durability
- [nginx](https://hub.docker.com/_/nginx) open source reverse proxy & load balancing for HTTP, HTTPS, SMTP, POP3 & IMAP protocols, HTTP cache and a web server
- [mariadb](https://hub.docker.com/_/mariadb) open source relational database by the original developers of MySQL and is much more efficient



## Summary

Docker desktop provides lots of tools to support local system integration work before code is sent to a continuous integration service (or as a temporary alternative if that CI service id down)

Practicalli Project Templates include `Dockerfile`, `.dockerignore` and `compose.yaml` configurations for Clojure development, kick-starting the use of Docker.

Docker images are a relatively clean way of trying out different services or even different operating systems, e.g.[Ubuntu](https://hub.docker.com/_/ubuntu) or [ArchLinux](https://hub.docker.com/_/archlinux).  Deleting the images removes the whole service without affecting the underlying operating system.

[MegaLinter](https://practical.li/engineering-playbook/code-quality/megalinter/#megalinter-configuration) is an excellent example of a docker image that provides a large number of tools that would otherwise need to be installed directly on the operating system.

- [Docker Desktop Overview](https://docs.docker.com/desktop/)
- [Docker Desktop Extensions overview](https://docs.docker.com/desktop/extensions/)
- [Docker Build overview](https://docs.docker.com/build/)
- [Docker Compose Overview](https://docs.docker.com/compose/)

Docker images are a relatively clean way of trying out different services or even different operating systems, e.g.[Debian](https://hub.docker.com/_/debian) or [ArchLinux](https://hub.docker.com/_/archlinux)
