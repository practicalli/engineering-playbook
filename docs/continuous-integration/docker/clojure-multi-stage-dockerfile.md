# Multi-Stage Dockerfile for Clojure

![Docker logo](https://github.com/practicalli/graphic-design/blob/live/topic-images/docker-logo-name.png?raw=true){align=right loading=lazy style="width:240px"}

Building a Clojure project from source to create a docker image provides an effective workflow for local testing and system integration.

A multi-stage Dockerfile is recommended to ensure build tools and artefacts are not included in the final Docker image, helping minimise the size and resources used by that image.

??? HINT "Practicalli Project Templates provide Multi-stage Dockerfile"
    [:globe_with_meridians: Practialli Project Templates](https://practical.li/clojure/clojure-cli/projects/templates/){target=_blank} provides `Dockerfile`, `.dockerignore` and `compose.yaml` configurations to optimise the build and run of Clojure projects.
    ```shell
    clojure -T:project/create :template practicalli/service :name practicalli/gameboard
    ```
    Practicalli Project Templates are included in the [:globe_with_meridians: Practicalli Clojure CLI Config](https://practical.li/clojure/clojure-cli/practicalli-config/){target=_blank} which provides aliases for running community tools to support a wide range of development tasks.

??? WARNING "Updating to Java 21 Long Term Support release"
    This guide will be updated to use images with Java 21, the latest Long Term Support release (LTS) as of October 2023.
    
     Examples will be provided using Alpine Linux and Debian Linux operating system. 


## Builder stage


Practicalli uses the latest Clojure CLI release and the latest Long Term Support (LTS) version of [:fontawesome-solid-book-open: Eclipse Temurin](images.md#openjdk) (OpenJDK).

[:fontawesome-solid-book-open: Alpine Linux](images.md#alpine-linux) image variants are used to keep the image file size as small as possible, reducing local resource requirements (and image download time).


!!! EXAMPLE "Set builder stage image using specific Java version"
    ```dockerfile title="Dockerfile"
    FROM clojure:temurin-21-tools-deps-alpine AS builder
    ```

> [clojure:temurin-21-tools-deps-alpine](https://hub.docker.com/layers/library/clojure/temurin-21-tools-deps-alpine/images/sha256-df9061b056300f3513407221ba4f5a60d330660563a6ecadcac62570a787bacd?context=explore) image details

`CLOJURE_VERSION` will over-ride the version of Clojure CLI in the Clojure image (which defaults to latest Clojure CLI release). Or choose an image that has a specific Clojure CLI version, e.g. `temurin-17-tools-deps-1.11.1.1182-alpine`


!!! EXAMPLE "Set builder stage image with specific Clojure version"
    ```dockerfile
    FROM clojure:temurin-17-alpine AS builder
    ENV CLOJURE_VERSION=1.11.1.1182
    ```

Create directory for building the project code and set it as the working directory within the Docker container to give RUN commands a path to execute from.

!!! EXAMPLE "Create working directory for build"
    ```dockerfile
    RUN mkdir -p /build
    WORKDIR /build
    ```

!!! INFO "Clojure CLI and tools.build"
    [:fontawesome-solid-book-open: Practicalli Clojure - Tools.Build](https://practical.li/clojure/clojure-cli/projects/package/tools-build/) is the recommended approach to creating an Uberjar from a Clojure CLI project.

    As long as the required files are copied, any build tool and task can be used to create the Uberjar file that packages the Clojure service for deployment.


### Cache Dependencies

Clojure CLI is used to download dependencies for the project and any other tooling used during the build stage, e.g. test runners, packaging tools to create an uberjar.  Dependency download should only occur once, unless the `deps.edn` file changes.

=== "Makefile"
    Copy the `deps.edn` file to the build stage and use the `clojure -P` prepare (dry run) command to download the dependencies without running any Clojure code or tools.

    !!! EXAMPLE "Create dependency cache overlay"
        ```dockerfile
        COPY deps.edn build.clj Makefile /build/
        RUN make deps
        ```
    The dependencies are cached in the Docker overlay (layer) and this cache will be used on successive docker builds unless the `deps.edn` file or `Makefile` is change.

=== "Clojure CLI"
    Copy the `deps.edn` file to the build stage and use the `clojure -P` prepare (dry run) command to download the dependencies without running any Clojure code or tools.

    !!! EXAMPLE "Create dependency cache overlay"
        ```dockerfile
        COPY deps.edn build.clj /build/
        RUN clojure -P -X:build
        ```
    The dependencies are cached in the Docker overlay (layer) and this cache will be used on successive docker builds unless the `deps.edn` file is change.

=== "Babashka Task Runner"
    Pull request welcome
    <!-- TODO Add babashka test runner for mutli-stage Dockerfile -->

`deps.edn` in this example contains the project dependencies and `:build` alias used build the Uberjar.

### Build Uberjar

Copy all the project files to the docker builder working directory, creating another overlay.

Copying the src and other files in a separate overlay to the `deps.edn` file ensures that changes to the Clojure code or configuration files does not trigger downloading of the dependencies again.


=== "Makefile"
    Run the `dist` task to generate an Uberjar for distribution.

    ```dockerfile
    COPY ./ /build
    RUN make dist
    ```

=== "Clojure CLI"
    Run the `tools.build` command to generate an Uberjar.

    ```dockerfile
    COPY ./ /build
    RUN clojure -T:build uberjar
    ```
    `:build` is an alias to include [Clojure tools.build](https://practical.li/clojure/clojure-cli/projects/tools-build/) dependencies which is used to build the Clojure project into an Uberjar.

=== "Babashka Task Runner"
    Pull request welcome

### Docker Ignore patterns

`.dockerignore` file in the root of the project defines file and directory patterns that Docker will ignore with the COPY command.  Use `.dockerignore` to avoid copying files that are not required for the build

Keep the `.dockerignore` file simple by excluding all files with `*` pattern and then use the `!` character to explicitly add files and directories that should be copied


=== "Makefile"
    !!! EXAMPLE "Docker ignore - only include specific patterns"
        ```shell
        # Ignore all files
        *

        # Include Clojure code and config
        !deps.edn
        !build.clj
        !Makefile
        !src/
        !test/
        !test-data/
        !resources/
        ```

=== "Clojure CLI"
    !!! EXAMPLE "Docker ignore - only include specific patterns"
        ```shell
        # Ignore all files
        *

        # Include Clojure code and config
        !deps.edn
        !src/
        !test/
        !test-data/
        !resources/
        ```

=== "Babashka Task Runner"
    Pull request welcome

`test-data` directory is commonly used by Practicalli to include scripts and data for testing the system once running.

> The classic approach for Docker ignoer patters is to [specify all files and directories to exclude in a Clojure project](https://gist.github.com/practicalli-john/36230953271cb0376a297d8a1d82ff6d), although this can lead to more maintenance as the project grows.


## Run-time stage

The Alpine Linux version of the Eclipse Temurin image is used as it is around 5Mb in size, compared to 60Mb or more of other operating system images.

!!! EXAMPLE "Image for run-time stage"
    ```dockerfile
    FROM eclipse-temurin:17-alpine AS final
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

!!! EXAMPLE "Add Alpine packages"
    ```dockerfile
    RUN apk add --no-cache \
        dumb-init~=1.2.5 \
        curl~=8.0.1 \
        jq~=1.6
    ```

> [Check Alpine packages](https://pkgs.alpinelinux.org/packages) if new major versions are no longer available (low frequency)

### Non-root group and user

Docker runs as root user by default and if a container is compromised the root permissions and could lead to a compromised host system. [Docker recommends creating a user and group in the run-time image](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user)  to run the service


!!! EXAMPLE "Create a non-privileged user account"
    ```dockerfile
    ARG UID=10001
    RUN adduser \
        --disabled-password \
        --gecos "" \
        --home "/nonexistent" \
        --shell "/sbin/nologin" \
        --no-create-home \
        --uid "${UID}" \
        clojure
    ```

!!! EXAMPLE "Create Non-root group and user"
    ```dockerfile
    RUN addgroup -S practicalli && adduser -S practicalli -G practicalli
    ```

!!! EXAMPLE "Create directory to contain service"
    Create directory to contain service archive, owned by non-root user
    ```dockerfile
    RUN mkdir -p /service && chown -R clojure. /service
    ```

!!! EXAMPLE "Set user"
    Instruct docker that all future commands should run as the appuser user
    ```dockerfile
    USER clojure
    ```

### Copy Uberjar to run-time stage

Create a directory to run the service or use a known existing path that will not clash with any existing files from the image.

Set the working directory and copy the uberjar archive file from Builder image

!!! EXAMPLE "Copy Uberjar from build stage"
    ```dockerfile
    WORKDIR /service
    COPY --from=builder --chown=clojure:clojure /build/practicalli-service.jar /service/practicalli-service.jar
    ```

Optionally, add system integration testing scripts to the run-time stage for testing from within the docker container.

```dockerfile
RUN mkdir -p /service/test-scripts
COPY --from=builder --chown=clojure:clojure /build/test-scripts/curl--* /service/test-scripts/
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

Clojure Uberjar runs on the Java Virtual Machine which is a highly optimised environment that rarely needs adjusting, unless there are noticeable performance or resource issue use.

Minimum and maximum heap sizes, i.e. `-XX:MinRAMPercentage` and `-XX:MaxRAMPercentage` are typically the only optimisations required.

`java -XshowSettings -version` displays VM settings (vm), Property settings (property), Locale settings (locale), Operating System Metrics (system) and the version of the JVM used.  Add the category name to show only a specific group of settings, e.g. `java -XshowSettings:system -version`.

`JDK_JAVA_OPTIONS` can be used to tailor the operation of the Java Virtual Machine, although the benefits and constraints of options should be well understood before using them (especially in production).

!!! EXAMPLE "Show settings on JVM startup"
    Show system settings on startup, force container mode and set memory heap maximum to 85% of host memory size.
    ```dockerfile
    ENV JDK_JAVA_OPTIONS "-XshowSettings:system -XX:+UseContainerSupport -XX:MaxRAMPercentage=85"
    ```

??? INFO "Use relative heap memory settings, not fixed sizes"
    Relative heap memory settings (`-XX:MaxRAMPercentage`) should be used for containers rather than the fixed value options (`-Xmx`) as the provisioning service for the container may control and change the resources available to a container on deployment (especially a Kubernettes system).

Options that are most relevant to running Clojure & Java Virtual Machine in a container include:

* `-XshowSettings:system` display (container) system resources on JVM startup
* `-XX:InitialRAMPercentage` Percentage of real memory used for initial heap size
* `-XX:MaxRAMPercentage` Maximum percentage of real memory used for maximum heap size
* `-XX:MinRAMPercentage` Minimum percentage of real memory used for maximum heap size on systems with small physical memory
* `-XX:ActiveProcessorCount` specifies the number of CPU cores the JVM should use regardless of container detection heuristics
* `-XX:±UseContainerSupport` force JVM to run in container mode, disabling container detection (only useful if JVM not detecting container environment)
* `-XX:+UseZGC` low latency Z Garbage collector (read the [Z Garbage collector documentation](https://docs.oracle.com/en/java/javase/17/gctuning/z-garbage-collector.html) and understand the trade-offs before use) - the default Hotspot garbage collector is the most effective choice for most services

??? WARNING "Only optimise if performance test data shows issues"
    Without performance testing of a specific Clojure service and analysis of the results, let the JVM use its own heuristics to determine the most optimum strategies it should use


### Expose Clojure service

If Clojure service listens to network requests when running, then the port it is listening on should be exposed so the world outside the container can communicate to the Clojure service.

e.g. expose port of HTTP Server that runs the Clojure service

!!! EXAMPLE "Expose service port"
    ```dockerfile
    EXPOSE 8080
    ```

### Entrypoint

Finally define how to run the Clojure service in the container.  The `java` command is used with the `-jar` option to run the Clojure service from the Uberjar archive.

The `java` command will use arguments defined in `JDK_JAVA_OPTIONS`.

ENTRYPOINT directive defines the command to run the service

```dockerfile
ENTRYPOINT ["java", "-jar", "/service/practicalli-service.jar"]
```

??? INFO "Docker ENTRYPOINT directive"
    `ENTRYPOINT` is the recommended way to run a service in Docker.  `CMD` can be used to pass additional arguments to the `ENTRYPOINT` command, or used instead of `ENTRYPOINT`.

    `jshell`is the default `ENTRYPOINT` for the Eclipse Temurin image. `jshell` will run if a `CMD` directive is not included in the run-time stage of the `Dockerfile`.

The `ENTRYPOINT` command runs as process id 1 (PID 1) inside the docker container.  In a Linux system PID 1 should respond to all TERM and SIGTERM signals.

[dump-init](https://github.com/Yelp/dumb-init) provides a simple process supervisor and init system, designed to run as PID 1 and manage all signals and child processes effectively.

Use `dumb-init` as the `ENTRYPOINT` command and `CMD` to pass the java command to start the Clojure service as an argument.  `dumb-init` ensures `TERM` signals are sent to the Java process and all child processes are cleaned up on shutdown.

!!! EXAMPLE "ENTRYPOINT to cleanly manage service"
    ```dockerfile
    ENTRYPOINT ["/usr/bin/dumb-init", "--"]
    CMD ["java", "-jar", "/service/practicalli-service.jar"]
    ```

> Alternatively, run dumb-jump and java within the `ENTRYPOINT` directive, `ENTRYPOINT ["/usr/bin/dumb-init", "--", "java", "-jar", "/service/practicalli-service.jar"]`.  This approach cannot be overridden correctly with an additional CMD directive on the command line when using `docker run`.

## Build and Run with docker

Ensure docker services are running, i.e. start docker desktop.

Build the service and create an image to run the Clojure service in a container with `docker build`.  Use a `--tag` to help identify the image and specify the context (in this example the root directory of the current project, `.`)

!!! NOTE ""
    ```bash
    docker build --tag practicalli/service-name:1.1 .
    ```

After the first time building the docker image, any parts of the build that havent changed will use their respective cached layers in the builder stage.  This can lead to very fast, even zero time builds.

![Docker build image optomised to use docker layer cache for build stage](https://raw.githubusercontent.com/practicalli/graphic-design/live/continuous-integration/docker-compose-build-output-cached-layers.png)

!!! HINT "Maximise use of Docker cache"
    Maximising the docker cache by careful consideration of command order and design in a `Dockerfile` can have a significant positive affect on build speed.

    Each command is an overlay (layer) in the Docker image and if its respective files have not changed, then the cached version of the command will be used.

Run the built image in a docker container using `docker run`, publishing the port number so it can be used from the host (developer environment or deployed environment).  Use the name of the image created by the tag in the docker build command.

!!! NOTE ""
    ```bash
    docker run --publish 8080:8080 practicalli/service-name
    ```

!!! HINT "Orchestrate multiple services with Compose"
    [Create a `compose.yml` file](compose.md) to defines all services to run to support local integration testing, optionally adding health checks and conditional starts.

    Run `docker compose up` to start all the services.
