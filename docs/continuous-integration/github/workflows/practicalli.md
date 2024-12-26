# Workflows for Practicalli

Practicalli books and other content websites use the following GitHub workflows.

## MegaLinter

[:fontawesome-solid-book-open: Practicalli MegaLinter workflow](megalinter.md){.md-button}

## Changelog Update Check

Check the CHANGELOG.md file has been updated for a pull request, providing a reminder to add a summary of changes for the pull request

Defines `changelog-check-skip` label on a pull request instructs the workflow not to run

!!! EXAMPLE "Changelog Checker"
    ```yaml title=".github/workflows/changelog-check.yml"
    ---
    # Check CHANGELOG.md file updated for every pull request

    name: Changelog Check
    on:
      pull_request:
        paths-ignore:
          - "README.md"
        types: [opened, synchronize, reopened, ready_for_review, labeled, unlabeled]

    jobs:
      changelog:
        name: Changelog Update Check
        runs-on: ubuntu-latest
        steps:
          - run: echo "🚀 Job automatically triggered by ${{ github.event_name }}"
          - run: echo "🐧 Job running on ${{ runner.os }} server"
          - run: echo "🐙 Using ${{ github.ref }} branch from ${{ github.repository }} repository"

          # Git Checkout
          - name: Checkout Code
            uses: actions/checkout@v3
            with:
              token: "${{ secrets.PAT || secrets.GITHUB_TOKEN }}"
          - run: echo "🐙 ${{ github.repository }} repository was cloned to the runner."

          # Changelog Enforcer
          - name: Changelog Enforcer
            uses: dangoslen/changelog-enforcer@v3
            with:
              changeLogPath: "CHANGELOG.md"
              skipLabels: "skip-changelog-check"

          # Summary and status
          - run: echo "🎨 Changelog Enforcer quality checks completed"
          - run: echo "🍏 Job status is ${{ job.status }}."
    ```

## Clojure Lint with Reviewdog

!!! EXAMPLE "clj-kondo lint with reviewdog reports"
    ```yaml
    ---
    # Clojure Lint with clj-kondo and reviewdog
    #
    # Lint errors raised as comments on pull request conversation

    name: Lint Review
    on: [pull_request]

    jobs:
      clj-kondo:
        name: runner / clj-kondo
        runs-on: ubuntu-latest
        steps:
          - run: echo "🚀 Job automatically triggered by ${{ github.event_name }}"
          - run: echo "🐧 Job running on ${{ runner.os }} server"
          - run: echo "🐙 Using ${{ github.ref }} branch from ${{ github.repository }} repository"

          # Git Checkout
          - name: Checkout Code
            uses: actions/checkout@v3
            with:
              token: "${{ secrets.PAT || secrets.GITHUB_TOKEN }}"
          - run: echo "🐙 ${{ github.repository }} repository was cloned to the runner."

          - name: clj-kondo
            uses: nnichols/clojure-lint-action@v2
            with:
              pattern: "*.clj"
              clj_kondo_config: ".clj-kondo/config-ci.edn"
              level: "error"
              exclude: ".cljstyle"
              github_token: ${{ secrets.github_token }}
              reporter: github-pr-review

          # Summary and status
          - run: echo "🎨 Lint Review checks completed"
          - run: echo "🍏 Job status is ${{ job.status }}."
    ```

## Clojure quality check

* clj-kondo syntax check for code and project configuration
* cljstyle code format check
* Kaocha unit test runner

!!! EXAMPLE "Clojure Quality Checks"
    ```yaml
    ---
    name: "Clojure Quality Check"

    on:
      pull_request:
      push:
        branches:
          - main

    jobs:
      tests:
        name: "Clojure Quality Checks"
        runs-on: ubuntu-latest
        steps:

          # Git Checkout
          - name: Checkout Code
            uses: actions/checkout@v3
          - run: echo "🐙 ${{ github.repository }} repository was cloned to the runner."

          - name: "Prepare Java runtime"
            uses: actions/setup-java@v3
            with:
              distribution: "temurin"
              java-version: "17"

          - name: "Cache Clojure Dependencies"
            uses: actions/cache@v3
            with:
              path: |
                ~/.m2/repository
                ~/.gitlibs
              key: clojure-deps-${{ hashFiles('**/deps.edn') }}
              restore-keys: clojure-deps-

          - name: "Install Clojure tools"
            uses: DeLaGuardo/setup-clojure@10
            with:
              cli: 1.11.1.1165 # Clojure CLI
              cljstyle: 0.15.0 # cljstyle
              clj-kondo: 2022.10.05 # Clj-kondo
              # bb: 0.7.8           # Babashka

          - name: "Lint with clj-kondo"
            run: clj-kondo --lint deps.edn src resources test --config .clj-kondo/config-ci.edn

          - name: "Check Clojure Style"
            run: cljstyle check --report

          - name: "Kaocha test runner"
            run: clojure -X:env/test:test/run
    ```

## mkdocs publisher

A workflow used to publish Practicalli books.

* `workflow_dispatch:` for manual trigger of workflow
* `workflow_run:` to depend on a successful run of the `MegaLinter` workflow
* `paths-ignore` defining paths to ignore changes from
* actions/setup-python installs python version 3
* `pip` to install Material for MkDocs packages used for Practialli books

!!! EXAMPLE "MkDocs Publish Book workflow"
    ```yaml
    ---
    name: Publish Book
    on:
      # Manually trigger workflow
      workflow_dispatch:

      # Run work flow conditional on linter workflow success
      workflow_run:
        workflows:
          - "MegaLinter"
        paths-ignore:
          - README.md
          - CHANGELOG.md
          - .gitignore
        branches:
          - main
        types:
          - completed

    permissions:
      contents: write

    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - run: echo "🚀 Job automatically triggered by ${{ github.event_name }}"
          - run: echo "🐧 Job running on ${{ runner.os }} server"
          - run: echo "🐙 Using ${{ github.ref }} branch from ${{ github.repository }} repository"

          - name: "Checkout code"
            uses: actions/checkout@v3
            with:
              fetch-depth: 0
          - run: echo "🐙 ${{ github.repository }} repository was cloned to the runner."

          - uses: actions/setup-python@v4
            with:
              python-version: 3.x
          - uses: actions/cache@v3
            with:
              key: ${{ github.ref }}
              path: .cache
          - run: pip install mkdocs-material mkdocs-callouts mkdocs-glightbox mkdocs-git-revision-date-localized-plugin mkdocs-redirects pillow cairosvg
          - run: mkdocs gh-deploy --force

          # Summary
          - run: echo "🎨 MkDocs book built and deployed to GitHub Pages"
          - run: echo "🍏 Job status is ${{ job.status }}."
    ```

## Scheduled Version Check

Use [liquidz/antq-action](https://github.com/liquidz/antq-action) to check for new versions of Clojure libraries and GitHub action.

The GtiHub action can use the following actions

* `excludes:` list of space separated artefact names to exclude from the version check, use `groupId/artifactId` for Java libraries
* `directories:` search paths to check, space separated.
* `skips:` project types to skip to search, space separated. One of boot, clojure-cli, github-action, pom, shadow-cljs or leiningen.

`on: schedule: cron:` is used to set the frequency for running the workflow, using a [POSIX cron syntax](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html#tag_20_25_07){target=_blank}.

[:fontawesome-brands-github: GitHub Docs: GitHub Actions - schedule](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule){target=_blank .md-button}

!!! EXAMPLE "Scheduled Antq Version check with Manual Trigger"
    ```yaml
    ---
    # ------------------------------------------
    # Scheduled check of versions
    # - use as non-urgent report on versions
    # - Uses POSIX Cron syntax
    #   - Minute [0,59]
    #   - Hour [0,23]
    #   - Day of the month [1,31]
    #   - Month of the year [1,12]
    #   - Day of the week ([0,6] with 0=Sunday)
    #
    # Using liquidz/anta to check:
    # - GitHub workflows
    # - deps.edn
    # ------------------------------------------

    name: "Scheduled Version Check"
    on:
      schedule:
        # - cron: "0 4 * * *" # at 04:04:04 ever day
        - cron: "0 4 * * 5" # at 04:04:04 ever Friday
        # - cron: "0 4 1 * *" # at 04:04:04 on first day of month
      workflow_dispatch: # Run manually via GitHub Actions Workflow page

    jobs:
      scheduled-version-check:
        name: "Scheduled Version Check"
        runs-on: ubuntu-latest
        steps:
          - run: echo "🚀 Job automatically triggered by ${{ github.event_name }}"
          - run: echo "🐧 Job running on ${{ runner.os }} server"
          - run: echo "🐙 Using ${{ github.ref }} branch from ${{ github.repository }} repository"

          - name: "Checkout code"
            uses: actions/checkout@v3
          - run: echo "🐙 ${{ github.repository }} repository was cloned to the runner."

          - name: "Antq Version Check"
            uses: liquidz/antq-action@main
            with:
              excludes: "org.clojure/tools.deps.alpha"
              # excludes: "qualifier/libary-name groupId/artifactId"
              # directories: "search/path/1 search/path/2"
              # skips: "boot clojure-cli github-action pom shadow-cljs leiningen"

          # Summary
          - run: echo "🎨 library versions checked with liquidz/antq"
          - run: echo "🍏 Job status is ${{ job.status }}."
    ```
