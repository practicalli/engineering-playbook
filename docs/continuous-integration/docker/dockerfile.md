# Dockerfile

A Dockerfile is a declarative configuration that defines how an image is assembled and can use another image to provide much of the configuration, minimising the amount of design and maintenance required.

Each line in the `Dockerfile` configuration creates an overlay, a layer of the image that is applied to create the final image.  Overlays are cached by docker on the first run and if the files or commands the overlays are created from do not change, then the cache is used on consecutive use of the docker image.

A [Multi-stage `Dockerfile`](clojure-multi-stage-dockerfile.md) is an effective way to build and run projects via a continuous integration pipeline and local development (in concert with supporting services via a [`compose.yaml` configuration](compose.md)).

[Docker Hub](https://hub.docker.com/_/amazoncorretto) provides a wide range of images, supporting development, continuous integration and system integration testing.


## Multi-stage Dockerfile

A multi-stage `Dockerfile` contains builder stage and an unnamed stage used as the run-time.  The builder stage can be designed optimally for building the Clojure project and the run-time stage optimised for running the service efficiently and securely.

The builder stage caches dependencies to optimise building Clojure and the run-time stage optimises running the service efficiently and securely.

The uberjar created by the builder image is copied over to the run-time image to keep that image as clean and small as possible (to minimise resource use).

![Docker Multi-stage build file with cached overlays](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-compose-build-output-cached-layers.png?raw=true)

??? "Practicalli Multi-stage Dockerfile for Clojure"
    [:fontawesome-solid-book-open: Practicalli Multi-stage `Dockerfile` for Clojure projects](clojure-multi-stage-dockerfile.md) derived from the configuration currently used for commercial and open source work.

    The Dockerfile uses make targets, which are Clojure commands defined in the [Practicalli Makefile](/engineering-playbook/build-tool/make/)


[Multi-stage `Dockerfile` for Clojure projects](clojure-multi-stage-dockerfile.md){.md-button}
[Docker Multi-stage builds docs](https://docs.docker.com/build/building/multi-stage/){target=_blank .md-button}

!!! WARNING "Docker init - beta feature"
    [`docker init`](https://docs.docker.com/engine/reference/commandline/init/) is a new (beta) feature to create `Dockerfile`, `.dockerignore` and`compose.yaml` files using Docker recommended practices.


[Docker Build overview](https://docs.docker.com/build/){target=_blank .md-button}


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

## Docker init

`docker init` provides a command line wizard to create set of Docker configurations that follow the recommended practices

```shell
docker init
```

Specific configurations are provides for Go and Python, other languages should use the General Purpose option.

The General purpose option provides almost all the configuration required.  Update the RUN command in the build stage to that of the specific language, ideally copying the project dependency file to the build stage and inititing a dry run to download the dependency files to create a Docker overlay cache of dependencies.


## Dockerfile Syntax

The form of `Dockerfile` instructions

```dockerfile
# Comment
INSTRUCTION arguments
```

`#` at the start of a line defines a line comment.  `#` within a line is considered an argument to the instruction.

`\` is a line continuation character, allowing a cleaner format when using longer commands as instruction arguments.

```dockerfile
HEALTHCHECK \
    CMD ["curl", "--fail", "http://localhost:8080/system-admin/status"]
```

Start by learning the common instructions used in `Dockerfile` configurations and consult [:globe_with_meridians: Docker Docs extensive Dockerfile reference](https://docs.docker.com/engine/reference/builder/){target=_blank} to understand additional instructions are required

[:globe_with_meridians: Docker Docs Dockerfile reference](https://docs.docker.com/engine/reference/builder/){target=_blank .md-button}


### FROM

`FROM` instruction should be the first instruction in the `Dockerfile` and specifies the image from which the current configuration uses as a base.

FROM can be preceded ARG instructions to declare arguments used in the `FROM` instruction.  Comments `#` and [parser directives](https://docs.docker.com/engine/reference/builder/#parser-directives){target=_blank} can also appear before `FROM`


### COPY

Copy files from the local directory or from a URL to the Docker images.

!!! INFO "COPY replaces ADD"
    Avoid the use of `ADD` and use `COPY` instead


### WORKDIR

Set the directory in the Docker image in which all future commands will run.

Setting `WORKDIR` can simplify `COPY` and `RUN` commands


### USER

Set user account to run all further commands


### RUN

Execute any command recongnised within the container


### HEALTHCHECK

Docker Service heathcheck provides a simple mechanism to report that a service is running.  A health check is a vital configuration to provide for production systems to ensure failing containers are replaced by working containers automatically.

The status of the service can be checked manually using the `docker inspect` command

!!! NOTE ""
```shell
docker inspect --format='{{json .State.Health}}' container-name
```

Define a `HEALTHCHECK CMD` to identify the current status of the service running within the container, e.g. a curl command to test an API endpoint that demonstrated the service is running.

Heathcheck options include

- `--interval` frequency to run the health check
- `--timeout` maximum time to wait for a response to the health check command
- `--start-period` grace period for the service to start up before the first health check runs
- `--retries` number of times to repeat the command before reporting failure (reports .... until success or failure)

Wait 10 seconds before running the first health check, then try every 5 seconds and wait 3 seconds for a response.  Run the health check a maximum of 2 times before reporting failure (assuming successful check has not been returned).

```dockerfile
--interval=5s --timeout=3s --start-period=10s --retries=2
```

!!! EXAMPLE "HEALTHCHECK using shell command"
    ```shell
    HEALTHCHECK \
      CMD curl --fail http://localhost:8080/system-admin/status || exit 1
    ```

!!! EXAMPLE "HEALTHCHECK using Docker Exec array command"
    ```dockerfile
    HEALTHCHECK \
        CMD ["curl", "--fail", "http://localhost:8080/system-admin/status"]
    ```

!!! HINT "Compose can also define a health check"
    `compose.yaml` can include a health check for each service defined which can be used by the service being built to conditionally start.

    Add a health-check to the `compose.yaml` configuration when the service being built requires a database or similar service that should be available before starting.

    Avoid defining a health-check in the `compose.yaml` for services that already have an adequate health-check defined in ther `Dockerfile`


### ENTRYPOINT

Define the default command to run once the container is started.

The image used to create the final stage from may have a default command, e.g. Eclipse Temurin image provides the `jshell` ENTRYPOINT, which is overridden by supplying an ENTRYPOINT in the `Dockerfile` for the current project.

`ENTRYPOINT` is typically used with `CMD`


!!! EXAMPLE "Process manager ENTRYPOINT with service CMD"
    Start dumb-init to manage the CMD process that calls the java run-time. `dumb-init` ensures SIGTERM signals are sent to the `java` process ensuring a clean shutdown
    ```dockerfile
    ENTRYPOINT ["/usr/bin/dumb-init", "--"]
    CMD ["java", "-jar", "/service/practicalli-suit-you-sir-standalone.jar"]
    ```

[:globe_with_meridians: Docker Entrypoint documentation](https://docs.docker.com/engine/reference/builder/#entrypoint){target=_blank .md-button}


### CMD

The command to run the service within the container, typically paired with ENTRYPOINT

CMD can be supplied on the command line when calling docker to over-ride the CMD defined in the `Dockerfile`, e.g. to help test a Docker image that is not running correctly or run variations of the service such as debug mode.

