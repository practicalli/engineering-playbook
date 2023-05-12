# Compose

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
