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

## Summary

A Multi-stage `Dockerfile` is an effective way of building and running Clojure projects, especially as the architecture grows in complexity.

Organising the commands in the `Dockerfile` to maximise the use of docker cache will speed up the build time by skipping tasks that would not change the resulting image.

Consider creating a `compose.yaml` file to orchestrate services that are required for development of the project and local system integration testing.
