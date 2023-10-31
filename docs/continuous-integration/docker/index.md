# Docker

Docker containers an isolated environment for repeatable build & runtime systems.

[:fontawesome-solid-book-open: Dockerfile](dockerfile.md) configuration defines a consistent approach to building the source code of project locally, or within a continuous integration (CI) system.

Designing a [:fontawesome-solid-book-open: `Dockerfile` configuration](dockerfile.md) to maximise use of image overlays (layers) greatly enhances the speed of the build, as libraries and other unchanged assets are cached on first run.

[:fontawesome-solid-book-open: Docker compose](compose.md) orchestrates multiple services locally. Heath checks and conditions can be set to ensure dependant services start in the correct order.

Docker compose can use a [:fontawesome-solid-book-open: `Dockerfile` configuration](dockerfile.md) to build a project when starting services. An `on-watch` feature can rebuild the project from sourcw when code changes are written to file. 


??? HINT "Docker and Clojure projects"
    A Docker Compose workflow complements the [Clojure REPL Driven Development workflow](https://practical.li/clojure/introduction/repl-workflow/).  

    A REPL connected editor provides instant feedback on the code as it is written, with Docker Compose providing a consistent build and orchestration of services.

    [:globe_with_meridians: Practicalli REPL Reloaded Workflow](https://practical.li/clojure/clojure-cli/repl-reloaded/){target=_blank .md-button}


## General Workflow

- [:fontawesome-solid-book-open: Install Docker Desktop](install.md) & [:fontawesome-solid-book-open: Extensions](desktop/extensions.md)
- [:fontawesome-solid-book-open: Select a Docker image](images.md) as a base or builder stage
- [:fontawesome-solid-book-open: Create a Dockerfile](dockerfile.md), e.g [:fontawesome-solid-book-open: a multi-stage build and run-time Dockerfile](clojure-multi-stage-dockerfile.md)
- [:fontawesome-solid-book-open: Compose services together](compose.md), adding health checks and conditional starts
- (optional) Automatic rebuild of Clojure project when [:fontawesome-solid-book-open: watching for code changes](compose.md#build-on-change) (experimental feature)

!!! HINT "Try the Docker getting started tutorial"
    Follow the Docker Getting Started tutorial from within Docker Desktop or via the command line.
    ```shell
    docker run -d -p 80:80 docker/getting-started
    ```

??? INFO "Practicalli Project Templates includes docker configuration"
    [Practicalli Project Templates](https://practical.li/clojure/clojure-cli/projects/templates/practicalli/) create projects that include a multi-stage `Dockerfile`, `.dockerignore` and `compose.yaml` configurations for Clojure development.

