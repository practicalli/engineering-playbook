# Compose

Docker Compose provides a declarative configuration for orchestrating services locally, providing a simple way to spin up services to try them out or orchestrate a whole system from numerous dependent services.

Compose can build images for Clojure projects and conditionally run based on the health check of other supporting services, e.g. Postgres Database.

Each service can define a heart beat which can be used as a conditional startup for other services which depend upon them.

## Compose Configuration

`compose.yaml` is the configuration file for Docker Compose

`docker compose` command will start all services defined in the `compose.yaml` configuration (or `--file filename` to specify an alternative configuration file).

```shell
docker compose up
```

- `--build` option will run the build process for services that contain a `build:` configuration
- `--detach` runs services in the background, compose startup logs are still shown in the foreground

Shutdown the services with `down` (specifying `--file filename` if used in the compose up command)

```shell
docker compose down
```

??? INFO "Compose simplified after Docker integration"
    `compose.yaml` is the new Compose configuration file for orchestrating services locally, a simplified and extended version of `docker-compose.yaml`.  The main benefit is that a version number is no longer required, as the Compose version shipped with Docker is used.

    `docker compose` the new command, replacing `docker-compose`, although the options and arguments to this command are the same for now.

## Build Clojure Service

The simplest approach to building a service is to include a `build:` configuration specifying the location of [a multi-stage `Dockerfile`](https://practical.li/engineering-playbook/continuous-integration/docker/clojure-multi-stage-dockerfile/), which has a `builder` stage to create an uberjar that is run in the run-time image.

The `build:` option for the Clojure service with the path to the multi-stage Dockerfile for the project (typically in the same root directory of the project, although a remote Git repository can also be used)

!!! EXAMPLE "Clojure project build configuration"
    ```yaml
    # --- Docker Compose Configuration --- #
    # - Docker Compose V2
    # - https://docs.docker.com/compose/compose-file/
    #
    # Build the Clojure Service from source code
    # and run on port 8080

    name: "practicalli"
    services:
      # --- Clojure Service --- #
      gameboard-service:
        platform: linux/amd64
        # Build using Dockerfile - relative path or Git repository
        build:
          context: ./ # Use Dockerfile in project root
        environment:
          - COMPOSE_PROJECT_NAME
        ports: # host:container
          - 8080:8080
    ```

* `name:` (optional) is used to set the container prefix of the service name, via the `environment:` variable `COMPOSE_PROJECT_NAME`, helping to identify the container by name when running.  The above container would be `practicalli-gameboard-service`.  This option is more valuable as the number of services grows
* `services:` contains one or more service definitions with a unique name, e.g. `gameboard-service`
* `gameboard-service:` is a unique name for a service configuration
* `platform:` defines the operating system and hardware architecture that should be used for the service
* `build: context:` defines the location of the `Dockerfile` to use, either a local file or a Git repository URL
* `ports:` defines the host:container mapping for the service port.  `8080:8000` value would map the service running within the container on `8000` to the host (e.g. engineers' computer) on port 8080.

![Docker Compose build process - Clojure project with tools.build](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-compose-build-in-progress-light.png?raw=true)


> Use [a multi-stage `Dockerfile`](https://practical.li/engineering-playbook/continuous-integration/docker/clojure-multi-stage-dockerfile/) to provide greater opportunity to create image overlays for the build stage which can be cached, e.g. downloading project dependencies, speeding up the build process on consecutive runs.

## Compose services

Add more unique service configurations under `services:`, e.g. `postgres-database`.  All the services that support the local testing and integration of the Clojure project can be added to the `compose.yaml` file (database, cache, mock APIs, coupled services, etc.).

The Clojure service defines a dependency on a Postgres Database.  The dependency has a condition so the Clojure service is only started once the Postgres service is healthy

!!! EXAMPLE "Compose Clojure project build with Postgres database"
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

* `depends_on:` include one or more services that the service depends on, optionally adding a start `condition:`
* `depends_on: <service-name> condition:` a condition that should be true in order for the service image to start, typically checking `service_healthy` of another service in the configuration
* `heathcheck:` define a command and options to determine if the specific service running is healthy, with the `test:` command specific to the type of service.


Build and run the services using the `--build` flag with `docker compose`

!!! NOTE ""
    ```shell
    docker compose up --build
    ```

The Clojure project is built in parallel with running the Postgres database.

On the initial run the Clojure project may take longer to run that starting the Postgres database, in which case the Clojure uberjar will run straight away.

On consecutive runs, at least part of the Clojure build process should be cached and images for all the services will have been downloaded and cached.  The Clojure uberjar may wait for the Postgres database to start up.

Each service can define a health check that can be used as a conditional startup trigger and ensure all services start in a meaningful order.

![Docker Compose startup - Clojure build and Postgres database - attache to database](https://github.com/practicalli/graphic-design/blob/live/continuous-integration/docker-compose-build-buld-clojure-run-postgres-healthy-attaching-light.png?raw=true)


### Build on Change

Docker provides [`watch`](https://docs.docker.com/compose/file-watch/) as an experimental feature which can rebuild the Clojure service when a file change is detected.

The watch approach seems most useful for Clojure projects when troubleshooting issues that occur during system integration testing.

When additional tools need to run outside of Clojure code, e.g. SaSS build, data loading / ETL, then `watch` can compliment the Clojure REPL workflow by providing incremental change management for non-clojur aspects of the project.

Add an `x-develop` configuration with `watch` under the Clojure service configuration.  Define the action for each path to create a simple but comprehensive way to update the service

!!! EXAMPLE "Clojure Project - Build on Change"
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

!!! HINT "Optimise Docker Cache in Build process"
    Using build on change approach can run quite frequently, so an optimised build process in the `Dockerfile` or `compose.yaml` configuration is especially important to make build on change fast and therefore effective to use.
