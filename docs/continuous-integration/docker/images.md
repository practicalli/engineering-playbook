# Docker Images

[Docker Official Images](https://docs.docker.com/docker-hub/official_images/) from Docker Hub are highly recommended.  Look for the Docker Official Image tag on the image page.

![Docker Official Image Tag](https://docs.docker.com/docker-hub/images/official-image-badge-iso.png)

- [Postgres](https://hub.docker.com/_/postgres) open-source object-relational database management system
- [Redis](https://hub.docker.com/_/redis) open-source, networked, in-memory, key-value data store with optional durability
- [nginx](https://hub.docker.com/_/nginx) open source reverse proxy & load balancing for HTTP, HTTPS, SMTP, POP3 & IMAP protocols, HTTP cache and a web server
- [mariadb](https://hub.docker.com/_/mariadb) open source relational database by the original developers of MySQL and is much more efficient

??? INFO "Docker Official Image meaning"
    An Official Docker Image means the configuration of that image follows the [Docker recommended practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/), is well documented and designed for common use cases.

    There is no implication as to the correctness of tools, languages or service that image provides, only in the means in which they are provided.

    However, if time was invested in creating an image good enough to pass the Docker review, then it has a higher probability of being a useful image that others that are not official.

## Alpine Linux

[:globe_with_meridians: Alpine Linux](https://www.alpinelinux.org/) images have a very small footprint and provide the essential tools for running services that do not depend on specific operating system packages.

As images are very small, resources used both locally and in stage and production environments are minimal.  This can be a simple way to reduce costs and do more with far fewer resources.

Alpine Linux uses the `pkg` tool for package management, to add tools and libraries to support a Dockerfile build stage if required.

## OpenJDK

OpenJDK is the most commonly used run-time environment for Java and JVM languages (Clojure, Kotlin, Gradle, Jython, JRuby, etc.)

[:globe_with_meridians: Eclipse temurin OpenJDK - official Docker image](https://hub.docker.com/_/eclipse-temurin) is built by the [:globe_with_meridians: Java community](https://adoptium.net/) and provides the Java run-time (Java Virtual Machine).  Eclipse Temurin provides all Long Term Support (LTS) versions from Java 8 onward and current intermediate releases.  Variants are available with Alpine Linux and a wide range of system architectures (`amd64`, `arm32v7`, `arm64v8`, `ppc64le`, `s390x`, `windows-amd64`)

### Amazon Corretto

[:globe_with_meridians: Amazon Corretto](https://hub.docker.com/_/amazoncorretto) is an OpenJDK distribution by Amazon AWS team and may be an appropriate choice if relying on Amazon support.

[Amazon Corretto](https://aws.amazon.com/corretto/) can also be installed for the local development environment, providing a consistent run-time between development and production.

## Clojure

[:globe_with_meridians: Clojure - official Docker Image](https://hub.docker.com/_/clojure/) - built by the Clojure community, provides tools to build Clojure projects (Clojure CLI, Leiningen)

Use the Clojure image within a `Dockerfile`, specifying `tools-deps` or `lein` variants

!!! EXAMPLE "Clojure CLI Docker Image as builder stage"
    ```dockerfile title="Dockerfile"
    FROM clojure:tools-deps AS builder
    ```

The Clojure image is built from the equivalent Eclipse Temurin image which can be used as the run-time image for a Multi-stage Dockerfile final stage.  As the build and final stages are built upon the same underlying image, a separate base stage is not required.

[:globe_with_meridians: Clojure - official Docker Image](https://hub.docker.com/_/clojure/){target=_blank .md-button}

!!! HINT "Multi-stage Dockerfile for Clojure"
