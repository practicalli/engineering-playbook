# Dockerfile

Deployment of Clojure is very simple, only an Uberjar archive file (Clojure Project and Clojure run-time) and the Java Run-time Environment (JRE) are required.

A Clojure service rarely works in isolation and although many services are access via a network connection (defined in Environment Variables), provisioning containers to build and run Clojure along with any other services can be valuable as complexity of the architecture grows.

A [Multi-stage `Dockerfile`](https://github.com/practicalli/clojure-app-template/blob/main/Dockerfile) is an effective way to build and run Clojure projects in continuous integration pipelines and during local development where multiple services are required for testing.

[Docker Hub](https://hub.docker.com/_/amazoncorretto) provides a wide range of images, supporting development, continuous integration and system integration testing.

## Multi-stage Dockerfile

A multi-stage `Dockerfile` contains builder stage and an unnamed stage used as the run-time.  The builder stage can be designed optimally for building the Clojure project and the run-time stage optimised for running the service efficiently and securely.

The uberjar created by the builder image is copied over to the run-time image to keep that image as clean and small as possible (to minimise resource use).

> [Example  Multi-stage `Dockerfile` for Clojure projects](https://github.com/practicalli/clojure-app-template/blob/main/Dockerfile) derived from the configuration currently used for commercial and open source work.  The example uses make targets, which are Clojure commands defined in the [example Makefile](https://github.com/practicalli/clojure-app-template/blob/main/Makefile)

## Official Docker images

Docker Hub contains a large variety of images, using those tagged with **Docker Official Image** is recommended.

* [Clojure - official Docker Image](https://hub.docker.com/_/clojure/) - provides tools to build Clojure projects (Clojure CLI, Leiningen, Boot)
* [Eclipse temurin OpenJDK - official Docker image](https://hub.docker.com/_/eclipse-temurin) - built by the [community](https://adoptium.net/) - provides the Java run-time

Ideally a base image should be used where both builder and run-time images share the same ancestor, this helps maintain consistency between build and run-time environments.

The Eclipse OpenJDK image is used by the Clojure docker image, so they implicitly use the same base image without needed to be specified in the project `Dockerfile`.  The Eclipse OpenJDK image could be used as a base image in the `Dockerfile` but it would mean repeating (and maintaining) much the work done by the official Clojure image)

Alternative Docker images
* [CircleCI Convenience Images => Clojure](https://hub.docker.com/r/cimg/clojure) - an optimised Clojure image for use with the [CircleCI service](https://circleci.com/)
* [Amazon Corretto](https://hub.docker.com/_/amazoncorretto) is an alternative version of OpenJDK

> An Official Docker Image means the configuration of that image follows the [Docker recommended practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/), is well documented and designed for common use cases.  There is no implication at to the correctness of tools, languages or service that image provides, only in the means in which they are provided.

## Clojure image as Builder stage

Practicalli uses the latest Clojure CLI release and the latest Long Term Support (LTS) version of Eclipse Temurin (OpenJDK).  Alpine Linux is used to keep the image file size as small as possible, reducing local resource requirements (and image download time).

```dockerfile
FROM clojure:temurin-17-alpine AS builder
```

`CLOJURE_VERSION` will over-ride the version of Clojure CLI in the Clojure image (which defaults to latest Clojure CLI release). Or choose an image that has a specific Clojure CLI version, e.g. `temurin-17-tools-deps-1.11.1.1182-alpine`

```dockerfile
FROM clojure:temurin-17-alpine AS builder
ENV CLOJURE_VERSION=1.11.1.1182
```

Create directory for building the project code and set it as the working directory within the Docker container to give RUN commands a path to execute from.

```dockerfile
RUN mkdir -p /build
WORKDIR /build
```

### Cache Dependencies

Clojure CLI is used to download dependencies for the project and any other tooling used during the build stage, e.g. test runners, packaging tools to create an uberjar.  Dependency download should only occur once, unless the `deps.edn` file changes.

Copy the `deps.edn` file to the build stage and use the `clojure -P` prepare (dry run) command to download the dependencies without running any Clojure code or tools.

```dockerfile
COPY deps.edn /build/
RUN clojure -P -X:build
```

The dependencies are cached in the Docker overlay (layer) and this cache will be used on successive docker builds unless the `deps.edn` file is change.

`deps.edn` in this example contains the project dependencies and `:build` alias used build the Uberjar.

### Build Uberjar

Copy all the project files to the docker builder working directory, creating another overlay.

Copying the src and other files in a separate overlay to the `deps.edn` file ensures that changes to the Clojure code or configuration files does not trigger downloading of the dependencies again.

Run the `tools.build` command to generate an Uberjar.

```dockerfile
COPY ./ /build
RUN clojure -T:build uberjar
```

`:build` is an alias to include [Clojure tools.build](https://practical.li/clojure/clojure-cli/projects/tools-build/) dependencies which is used to build the Clojure project into an Uberjar.

> [When using make for the build](https://practical.li/blog/posts/make-clojure-tasks-simple-and-consistent/), then also copy the `Makefile` to the builder stage and call the `deps` target to download the dependencies (ensure `deps` target in the `Makefile` depends on the `deps.edn` file so the target is skipped if that file has not changed)
> ```dockerfile
> COPY deps.edn Makefile /build/
> RUN make deps
> ```
> call the `dist` target to build the Uberjar
> ```dockerfile
> COPY ./ /build
> RUN RUN make dist
> ```

### Docker Ignore patterns

`.dockerignore` file in the root of the project defines file and directory patterns that Docker will ignore with the COPY command.  Use `.dockerignore` to avoid copying files that are not required for the build

Keep the `.dockerignore` file simple by excluding all files with `*` pattern and then use the `!` character to explicitly add files and directories that should be copied

```
# Ignore all files
*

# Include Clojure code and config
!deps.edn
!Makefile
!src/
!test/
!test-data/
!resources/
```

`Makefile` and `test-data` directories are commonly used by Practicalli, although in general are not widely needed.

> The classic approach would be to [specify all files and directories to exclude in a Clojure project](https://gist.github.com/practicalli-john/36230953271cb0376a297d8a1d82ff6d), although this typically means more maintenance


## OpenJDK for Run-time stage

The Alpine Linux version of the Eclipse Temurin image is used as it is around 5Mb in size, compared to 60Mb or more of other operating system images.

```dockerfile
FROM eclipse-temurin:17-alpine
```

Run-time containers are often cached in a repository, e.g. AWS Container Repository (ECR).  `LABEL` adds metadata to the container helping it to be identified in a repository or in a local development environment.

```dockerfile
LABEL org.opencontainers.image.authors="nospam+dockerfile@practicall.li"
LABEL io.github.practicalli.service="Gameboard API Service"
LABEL io.github.practicalli.team="Practicalli Engineering Team"
LABEL version="1.0"
LABEL description="Gameboard API service"
```

> Use `docker inspect` to view the metadata

Optionally, add packages to support running the service or helping to debug issue in the container when it is running.  For example, add `dumb-init` to manage processes, `curl` and `jq` binaries for manual running of system integration testing scripts for API testing.

`apk` is the package tool for Alpine Linux and `--no-cache` option ensures the install file is not part of the resulting image, saving resources.  Alpine Linux recommends setting versions to use any point release with the `~=` approximately equal version, so any same major.minor version of the package can be used.

```dockerfile
RUN apk add --no-cache \
    dumb-init~=1.2.5 \
    curl~=8.0.1 \
    jq~=1.6
```

> [Check Alpine packages](https://pkgs.alpinelinux.org/packages) if new major versions are no longer available (low frequency)

### Non-root group and user 

Docker runs as root user by default and if a container is compromised the root permissions and could lead to a compromised system.  Add a user and group to the run-time image and create a directory to contain service archive, owned by the non-root user.  Then instruct docker that all future commands should run as the non-root user

```dockerfile
RUN addgroup -S practicalli && adduser -S practicalli -G practicalli
RUN mkdir -p /service && chown -R practicalli. /service
USER practicalli
```

### Copy Uberjar to run-time stage

Create a directory to run the service or use a known existing path that will not clash with any existing files from the image.

Set the working directory and copy the uberjar archive file from Builder image

```dockerfile
RUN mkdir -p /service
WORKDIR /service
COPY --from=builder /build/practicalli-service.jar /service/practicalli-service.jar
```

Optionally, add system integration testing scripts to the run-time stage for testing from within the docker container.

```dockerfile
RUN mkdir -p /service/test-scripts
COPY --from=builder /build/test-scripts/curl--* /service/test-scripts/
```

### Service Environment variables

Define values for environment variables should they be required (usually for debugging), ensuring no sensitive values are used. Environment variables are typically set by the service provisioning the containers (AWS ECS / Kubernettes) or on the local OS host during development (Docker Desktop).

```dockerfile
# optional over-rides for Integrant configuration
# ENV HTTP_SERVER_PORT=
# ENV MYSQL_DATABASE=
ENV SERVICE_PROFILE=prod
```

### Java Virtual Machine Optimisation

Clojure Uberjar runs on the Java Virtual Machine which is a highly optimised environment that rarely needs adjusting, unless there are noticeable performance or resource issue use.  The most likely option to set is the minimum and maximum heap sizes, i.e. `-XX:MinRAMPercentage` and `-XX:MaxRAMPercentage`.

`java -XshowSettings -version` displays VM settings (vm), Property settings (property), Locale settings (locale), Operating System Metrics (system) and the version of the JVM used.  Add the category name to show only a specific group of settings, e.g. `java -XshowSettings:system -version`.

`JDK_JAVA_OPTIONS` can be used to tailor the operation of the Java Virtual Machine, although the benefits and constraints of options should be well understood before using them (especially in production).

Example: show system settings on startup, force container mode and set memory heap maximum to 85% of host memory size.

```dockerfile
ENV JDK_JAVA_OPTIONS "-XshowSettings:system -XX:+UseContainerSupport -XX:MaxRAMPercentage=85"
```

> Relative heap memory settings (`-XX:MaxRAMPercentage`) should be used for containers rather than the fixed value options (`-Xmx`) as the provisioning service for the container may control and change the resources available to a container on deployment (especially a Kubernettes system).

Options that are most relevant to running Clojure & Java Virtual Machine in a container include:

* `-XshowSettings:system` display (container) system resources on JVM startup
* `-XX:InitialRAMPercentage` Percentage of real memory used for initial heap size
* `-XX:MaxRAMPercentage` Maximum percentage of real memory used for maximum heap size
* `-XX:MinRAMPercentage` Minimum percentage of real memory used for maximum heap size on systems with small physical memory
* `-XX:ActiveProcessorCount` specifies the number of CPU cores the JVM should use regardless of container detection heuristics
* `-XX:±UseContainerSupport` force JVM to run in container mode, disabling container detection (only useful if JVM not detecting container environment)
* `-XX:+UseZGC` low latency Z Garbage collector (read the [Z Garbage collector documentation](https://docs.oracle.com/en/java/javase/17/gctuning/z-garbage-collector.html) and understand the trade-offs before use) - the default Hotspot garbage collector is the most effective choice for most services

> Without performance testing of a specific Clojure service and analysis of the results, let the JVM use its own heuristics to determine the most optimum strategies it should use


### Expose Clojure service

If Clojure service listens to network requests when running, then the port it is listening on should be exposed so the world outside the container can communicate to the Clojure service.

e.g. expose port of HTTP Server that runs the Clojure service

```dockerfile
EXPOSE 8080
```

### Entrypoint

Finally define how to run the Clojure service in the container.  The `java` command is used with the `-jar` option to run the Clojure service from the Uberjar archive.

The `java` command will use arguments defined in `JDK_JAVA_OPTIONS` automatically.

ENTRYPOINT directive defines the command to run the service

```dockerfile
ENTRYPOINT ["java", "-jar", "/service/practicalli-service.jar"]
```

> `ENTRYPOINT` is the recommended way to run a service in Docker.  `CMD` can be used to pass additional arguments to the `ENTRYPOINT` command, or used instead of `ENTRYPOINT`.
>
> `jshell`is the default `ENTRYPOINT` for the Eclipse Temurin image, so `jshell` will run if an `ENTRYPOINT` of `CMD` directive is not included in the run-time stage of the `Dockerfile`.

The `ENTRYPOINT` command runs as process id 1 (PID 1) inside the docker container.  In a Linux system PID 1 should respond to all TERM and SIGTERM signals.

[dump-init](https://github.com/Yelp/dumb-init) provides a simple process supervisor and init system, designed to run as PID 1 and manage all signals and child processes effectively.

Use `dumb-init` as the `ENTRYPOINT` command and `CMD` to pass the java command to start the Clojure service as an argument.  `dumb-init` ensures `TERM` signals are sent to the Java process and all child processes are cleaned up on shutdown.

```dockerfile
ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["java", "-jar", "/service/practicalli-service.jar"]
```

> Alternatively, run dumb-jump and java within the `ENTRYPOINT` directive, `ENTRYPOINT ["/usr/bin/dumb-init", "--", "java", "-jar", "/service/practicalli-service.jar"]`

## Build and Run with docker

Ensure docker services are running, i.e. start docker desktop.

Build the service and create an image to run the Clojure service in a container with `docker build`.  Use a `--tag` to help identify the image and specify the context (in this example the root directory of the current project, `.`)

```bash
docker build --tag practicalli/service-name:1.1 .
```

After the first time building the docker image, any parts of the build that havent changed will use their respective cached layers in the builder stage.  This can lead to very fast, even zero time builds.

![Docker build image optomised to use docker layer cache for build stage](https://raw.githubusercontent.com/practicalli/graphic-design/live/continuous-integration/docker-compose-build-output-cached-layers.png)

> Maximising the docker cache by careful consideration of command order and design in a `Dockerfile` can have a significant positive affect on build speed. Each command is effectively a layer in the Docker image and if its respective files have not changed, then the cached version of the command will be run

Run the built image in a docker container using `docker run`, publishing the port number so it can be used from the host (developer environment or deployed environment).  Use the name of the image created by the tag in the docker build command.

```bash
docker run --publish 8080:8080 practicalli/service-name
```

> Consider creating a `docker-compose.yml` file that defines all the services that should be run to support local development, then run `docker compose up` to start all the services.


## Summary

A Multi-stage `Dockerfile` is an effective way of building and running Clojure projects, especially as the architecture grows in complexity.

Organising the commands in the `Dockerfile` to maximise the use of docker cache will speed up the build time by skipping tasks that would not change the resulting image.

Consider creating a `compose.yaml` file to orchestrate services that are required for development of the project and local system integration testing.

