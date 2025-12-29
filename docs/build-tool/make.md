# Make

[GNU Make](https://www.gnu.org/software/make/) provide a simple and consistent way to run any development task for Clojure & ClojureScript projects (or any other languages).

Wrap any combination of tools (building, linting, formatting, testing) with make targets for a simple command line interface, with automatically tab completion, making any Clojure project really easy to work with.  Practicalli also uses make to manage docker images and containers to support Clojure development.

All that is required is a [:fontawesome-brands-github: `Makefile` in the root of the project](https://github.com/practicalli/dotfiles/blob/main/Makefile)

??? EXAMPLE "Practicalli Makefile for Clojure projects"
    ```yaml title=".github/config/megalinter.yaml"
    # ------------------------------------------
    # Makefile: Clojure Service
    #
    # Consistent set of targets to support local development of Clojure
    # and build the Clojure service during CI deployment
    #
    # Requirements
    # - cljstyle
    # - Clojure CLI aliases
    #   - `:env/dev` to include `dev` directory on class path
    #   - `:env/test` to include `test` directory and libraries to support testing
    #   - `:test/run` to run kaocha kaocha test runner and supporting paths and dependencies
    #   - `:repl/rebel` to start a Rebel terminal UI
    #   - `:package/uberjar` to create an uberjar for the service
    # - docker
    # - mega-linter-runner
    #
    # ------------------------------------------

    # .PHONY: ensures target used rather than matching file name
    # https://makefiletutorial.com/#phony
    .PHONY: all lint deps dist pre-commit-check repl test test-ci test-watch clean

    # ------- Makefile Variables --------- #

    # run help if no target specified
    .DEFAULT_GOAL := help

    # Column the target description is printed from
    HELP-DESCRIPTION-SPACING := 24

    # Makefile file and directory name wildcard
    # EDN-FILES := $(wildcard *.edn)
    # ------------------------------------ #

    # ------- Help ----------------------- #
    # Source: https://nedbatchelder.com/blog/201804/makefile_help_target.html

    help:  ## Describe available tasks in Makefile
        @grep '^[a-zA-Z]' $(MAKEFILE_LIST) | \
        sort | \
        awk -F ':.*?## ' 'NF==2 {printf "\033[36m  %-$(HELP-DESCRIPTION-SPACING)s\033[0m %s\n", $$1, $$2}'
    # ------------------------------------ #

    # ------- Clojure Development -------- #

    repl:  ## Run Clojure REPL with rich terminal UI (Rebel Readline)
        $(info --------- Run Rebel REPL ---------)
        clojure -M:test/env:repl/reloaded


    deps: deps.edn  ## Prepare dependencies for test and dist targets
        $(info --------- Download test and service libraries ---------)
        clojure -P -X:build


    dist: build-uberjar ## Build and package Clojure service
        $(info --------- Build and Package Clojure service ---------)


    # Remove files and directories after build tasks
    # `-` before the command ignores any errors returned
    clean:  ## Clean build temporary files
        $(info --------- Clean Clojure classpath cache ---------)
        - rm -rf ./.cpcache
    # ------------------------------------ #

    # ------- Testing -------------------- #

    test-config:  ## Print Kaocha test runner configuration
            $(info --------- Runner Configuration ---------)
            clojure -M:test/env:test/run --print-config

    test-profile:  ## Profile unit test speed, showing 3 slowest tests
            $(info --------- Runner Profile Tests ---------)
            clojure -M:test/env:test/run --plugin  kaocha.plugin/profiling

    test:  ## Run unit tests - stoping on first error
            $(info --------- Runner for unit tests ---------)
            clojure -X:test/env:test/run


    test-all:  ## Run all unit tests regardless of failing tests
            $(info --------- Runner for all unit tests ---------)
            clojure -X:test/env:test/run :fail-fast? false

    test-watch:  ## Run tests when changes saved, stopping test run on first error
            $(info --------- Watcher for unit tests ---------)
            clojure -X:test/env:test/run :watch? true

    test-watch-all:  ## Run all tests when changes saved, regardless of failing tests
            $(info --------- Watcher for unit tests ---------)
            clojure -X:test/env:test/run :fail-fast? false :watch? true
    # ------------------------------------ #


    # -------- Build tasks --------------- #
    build-config: ## Pretty print build configuration
        $(info --------- View current build config ---------)
        clojure -T:build config

    build-jar: ## Build a jar archive of Clojure project
        $(info --------- Build library jar ---------)
        clojure -T:build jar

    build-uberjar: ## Build a uberjar archive of Clojure project & Clojure runtime
        $(info --------- Build service Uberjar  ---------)
        clojure -T:build uberjar

    build-clean: ## Clean build assets or given directory
        $(info --------- Clean Build  ---------)
        clojure -T:build clean
    # ------------------------------------ #

    # ------- Code Quality --------------- #

    pre-commit-check: format-check lint test  ## Run format, lint and test targets

    format-check: ## Run cljstyle to check the formatting of Clojure code
        $(info --------- cljstyle Runner ---------)
        cljstyle check

    format-fix:  ## Run cljstyle and fix the formatting of Clojure code
        $(info --------- cljstyle Runner ---------)
        cljstyle fix

    lint:  ## Run MegaLinter with custom configuration (node.js required)
        $(info --------- MegaLinter Runner ---------)
        npx mega-linter-runner --flavor java --env "'MEGALINTER_CONFIG=.github/config/megalinter.yaml'" --remove-container

    lint-fix:  ## Run MegaLinter with applied fixes and custom configuration (node.js required)
        $(info --------- MegaLinter Runner ---------)
        npx mega-linter-runner --fix --flavor java --env "'MEGALINTER_CONFIG=.github/config/megalinter.yaml'" --remove-container

    lint-clean:  ## Clean MegaLinter report information
        $(info --------- MegaLinter Clean Reports ---------)
        - rm -rf ./megalinter-reports
    # ------------------------------------ #

    # ------- Docker Containers ---------- #

    docker-build:  ## Build Clojure project and run with docker compose
        $(info --------- Docker Compose Build ---------)
        docker compose up --build --detach

    docker-build-clean:  ## Build Clojure project and run with docker compose, removing orphans
        $(info --------- Docker Compose Build - remove orphans ---------)
        docker compose up --build --remove-orphans --detach

    docker-down:  ## Shut down containers in docker compose
        $(info --------- Docker Compose Down ---------)
        docker compose down


    swagger-editor:  ## Start Swagger Editor in Docker
        $(info --------- Run Swagger Editor at locahost:8282 ---------)
        docker compose -f swagger-editor.yml up -d swagger-editor

    swagger-editor-down:  ## Stop Swagger Editor in Docker
        $(info --------- Run Swagger Editor at locahost:8282 ---------)
        docker compose -f swagger-editor.yml down
    # ------------------------------------ #

    # ------ Continuous Integration ------ #

    # .DELETE_ON_ERROR: halts if command returns non-zero exit status
    # https://makefiletutorial.com/#delete_on_error

    test-ci: deps  ## Test runner for integration tests
        $(info --------- Runner for integration tests ---------)
        clojure -P -X:test/env:test/run

    # Run tests, build & package the Clojure code and clean up afterward
    # `make all` used in Docker builder stage
    .DELETE_ON_ERROR:
    all: test-ci dist clean  ## Call test-ci dist and clean targets, used for CI
    ```

## GNU Make overview

[GNU Make](https://www.gnu.org/software/make/){target=_blank} is a programming language agnostic build automation tool which has been an integral part of building Linux/Unix operating system code and applications for decades, providing a consistent way to configure, compile and deploy code for all projects.

A `Makefile` defines targets called via the `make` command. Each target can run one or more commands.  Targets can be dependent on other targets,  e.g the `dist` target that builds a project can be dependent on `deps` & `test` targets.

[GNU Make](https://www.gnu.org/software/make/){target=_blank} is available on all Linux/Unix based operating systems (and Windows via [chocolatey](https://chocolatey.org/){target=_blank} or nmake).

> Practicalli also uses `make` to [configure and build the latest versions of Emacs](https://practical.li/blog/posts/build-emacs-28-on-ubuntu/){target=_blank} and other Linux open source software

## Defining tasks

Create a `Makefile` in the root of a project and define a target by typing a suitable name followed by a `:` character, e.g. `test:`

Insert a tab on the next line and type a command to be called.  Further commands can be added on new lines so long as each line is tab indented.

The `repl` target prints out an information message and then uses the [Clojure CLI](https://clojure.org/guides/install_clojure){target=_blank} with aliases from [Practicalli Clojure CLI Config](https://practical.li/clojure/clojure-cli/practicalli-config/){target=_blank} to run a Clojure REPL process with a rich terminal UI ([Rebel Readline](https://github.com/bhauman/rebel-readline))

```makefile
repl:  ## Run Clojure REPL with rich terminal UI (Rebel Readline)
    $(info --------- Run Rebel REPL ---------)
    clojure -M:env/dev:env/test:repl/rebel
```

[:fontawesome-solid-book-open: Common task naming convensions](/engineering-playbook/build-tool/#common-task-naming){target=_blank .md-button}


## Target dependencies

A `Makefile` target can depend on either a file name or another target in the `Makefile`.

The all target typically depends on several `Makefile` targets to test, compile and package a service.  Add the names of the targets this target depends upon

```makefile
all: deps test-ci dist clean
```

Add the filename of a file after the name of the target, to depend on if that file has changed.  If the file has not changed since make was last run then the task will not run again.

Clojure CLI Example: If the `deps` target depends on `deps.edn` and the file has not changed since last run, the deps target will not run again.

## deps target - depend on a file

The deps target would use Clojure CLI or Leiningen to download dependencies.

Configuring the `deps` target to depend on `deps.edn` or `project.clj` file, then if the file has not changed the deps will not run again.

A Clojure CLI example depends on the `deps.edn` file that defines all the library dependencies for the project, tools for testing and packaging the Clojure service.  The `-P` flag is the prepare option, a dry run that only downloads the dependencies for the given tasks.

```makefile
deps: deps.edn  ## Prepare dependencies for test and dist targets
    $(info --------- Download libraries to test and build the service ---------)
    clojure -P -X:env/test:package/uberjar
```

> `:env/test` adds libraries to run Kaocha and libraries used to run unit tests.  `:package/uberjar` runs a tool that creates an uberjar.

## Clean target - hiding command failure

The clean target should remove files and directories created by the build (compile) process, to ensure a consistent approach to building each time.

On Linux / Unix systems files can be deleted with the `rm` command using `-r` for recursively deleting a directory and its contents. `-f` forces the deleting of files and directories, otherwise a prompt for confirmation of the delete may be shown.

`-` before a command  instructs `make` to ignore an error code, useful if the files to be deleted did not exist (i.e. the build failed part way through and not all files were created).

```makefile
# `-` before the command ignores any errors returned
clean:
    $(info --------- Clean Clojure classpath cache ---------)
    - rm -rf ./.cpcache
```

## MegaLinter target - simplifying a command

The `lint` target is an example of how the `Makefile` simplifies the command line interface.

`lint:` target is used to call the MegaLinter runner, avoiding the need to remember the common options passed when calling MegaLinter command line tool, `mega-linter-runner`

The Java flavor of MegaLinter is less than 2Gb image (full MegaLinter image is 8Gb) and contains all the tools for a Clojure project.  The flavor can only be set via a command line option, so the make file ensures that option is always used and the full MegaLinter docker image is not downloaded by mistake.

When MegaLinter is configured to generate reports (default), `lint-clean:` target is used to clean those reports.

```makefile
# Run MegaLinter with custom configuration
lint:
    $(info --------- MegaLinter Runner ---------)
    mega-linter-runner --flavor java --env 'MEGALINTER_CONFIG=.github/linters/mega-linter.yml'

lint-clean:
    $(info --------- MegaLinter Clean Reports ---------)
    - rm -rf ./megalinter-reports
```

## Enhancing make output

The `info` message is used with each target to enhances the readability of the make output, especially when multiple targets and commands are involved, or if commands are generating excessive output to standard out.

```makefile
 test:
    $(info --------- Runner for unit tests ---------)
    ./bin/test
```

![Makefile showing make all output with info messages to separate the Clojure commands and output](https://raw.githubusercontent.com/practicalli/graphic-design/live/clojure/makefile-clojure-make-all-output-with-info.png)

## Avoiding file name collisions

Although unlikely, if a filename in the root of a project has the same name as a `Makefile` target, it can be used instead of running the targets command

`.PHONY:` defines the names of targets in the `Makefile` to avoid name clashes

```makefile
.PHONY: all lint deps test test-ci dist clean
```

> Reference: [:fontawesome-solid-book-open: Makefile Tutorial: phony](https://makefiletutorial.com/#phony){target=_blank .md-button}

## Halt on failure

`.DELETE_ON_ERROR:` halts any further commands if a command returns non-zero exit status.  Useful as short-circuit to stop tasks when further work is not valuable, e.g. if tests fail then it may not be valuable to build the Clojure project.

```makefile
.DELETE_ON_ERROR:
all: deps test-ci dist clean
```

> Reference: [:fontawesome-solid-book-open: Makefile Tutorial:  delete_on_error](https://makefiletutorial.com/#delete_on_error){target=_blank .md-button}

## References

[:fontawesome-solid-book-open: Makefile Tutorial by Example](https://makefiletutorial.com/){target=_blank .md-button}
[:fontawesome-brands-github: practicalli/dotfiles Makefile](https://github.com/practicalli/dotfiles/blob/main/Makefile){target=_blank .md-button}

## Summary

A `Makefile` can simplify the command line interface for any task with a Clojure project (or any other language and tooling).

Using the same target names across all projects reduces the cognitive load for driving any project.
