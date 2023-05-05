# GitHub workflows

GitHub workflows used by Practicalli


!!! HINT "Use major version only"
    Use the major version of a GitHub action within a GitHub workflows to minimise the maintenance of action versions in workflow configuration.  e.g. if the latest version is `v5.2.1`, then use version `v5` to use the latest version available within that major version.


## Backstage.io

!!! EXAMPLE "Validate Backstage.io configuration files"
    ```yaml
    ---
    # --- Validate Backstage.io configuration files ---#
    # https://github.com/marketplace/actions/backstage-entity-validator

    # trigger workflow if yaml files `.backstage/` directory updated
    name: Backstage Validator
    on:
      pull_request:
        paths:
          - ".backstage/"

    jobs:
      # Validate backstage configuration
      backstage-validator:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3
          - uses: RoadieHQ/backstage-entity-validator@v0.3.2
            with:
              path: ".backstage/*.yaml"
    ```


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
          - "CHANGELOG.md"
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

## MegaLinter

!!! EXAMPLE "Code and configuration file lint checks"
    ```yaml title=".github/workflow/megalinter.yaml"
    ---
    # MegaLinter GitHub Action configuration file
    # More info at https://megalinter.github.io
    # All variables described in https://megalinter.github.io/configuration/

    name: MegaLinter
    on:
      workflow_dispatch:
      pull_request:
        branches: [main]
      push:
        branches: [main]

    env:
      # Apply linter fixes configuration
      APPLY_FIXES: all # APPLY_FIXES must be defined as environment variable
      APPLY_FIXES_EVENT: pull_request # events that trigger fixes on a commit or PR (pull_request, push, all)
      APPLY_FIXES_MODE: pull_request # are fixes are directly committed (commit) or posted in a PR (pull_request)

    # Run Linters in parallel
    # Cancel running job if new job is triggered
    concurrency:
      group: "${{ github.ref }}-${{ github.workflow }}"
      cancel-in-progress: true

    jobs:
      megalinter:
        name: MegaLinter
        runs-on: ubuntu-latest
        steps:
          - run: echo "🚀 Job automatically triggered by ${{ github.event_name }}"
          - run: echo "🐧 Job running on ${{ runner.os }} server"
          - run: echo "🐙 Using ${{ github.ref }} branch from ${{ github.repository }} repository"

          # Message on first interaction
          - name: First interaction
            uses: actions/first-interaction@v1
            with:
              # Token for the repository
              repo-token: "{{ secrets.GITHUB_TOKEN }}"
              # Comment to post on an individual's first issue
              issue-message: "[Practicalli Contributing Guide](https://practical.li/spacemacs/introduction/contributing/)"
              # Comment to post on an individual's first pull request
              pr-message: "[Practicalli Contributing Guide](https://practical.li/spacemacs/introduction/contributing/)"

          # Git Checkout
          - name: Checkout Code
            uses: actions/checkout@v3
            with:
              token: "${{ secrets.PAT || secrets.GITHUB_TOKEN }}"
              fetch-depth: 0
          - run: echo "🐙 ${{ github.repository }} repository was cloned to the runner."

          # MegaLinter Configuration
          - name: MegaLinter Run
            id: ml
            ## latest release of major version
            uses: oxsecurity/megalinter/flavors/java@v6
            env:
              MEGALINTER_CONFIG: .github/config/megalinter.yaml
              GITHUB_TOKEN: "${{ secrets.GITHUB_TOKEN }}" # report individual linter status
              # Validate all source when push on main, else just the git diff with live.
              VALIDATE_ALL_CODEBASE: >-
                ${{ github.event_name == 'push' && github.ref == 'refs/heads/main'}}


          # Upload MegaLinter artifacts
          - name: Archive production artifacts
            if: ${{ success() }} || ${{ failure() }}
            uses: actions/upload-artifact@v2
            with:
              name: MegaLinter reports
              path: |
                megalinter-reports
                mega-linter.log

          # Create pull request if applicable (for now works only on PR from same repository, not from forks)
          - name: Create Pull Request with applied fixes
            id: cpr
            if: steps.ml.outputs.has_updated_sources == 1 && (env.APPLY_FIXES_EVENT == 'all' || env.APPLY_FIXES_EVENT == github.event_name) && env.APPLY_FIXES_MODE == 'pull_request' && (github.event_name == 'push' || github.event.pull_request.head.repo.full_name == github.repository) && !contains(github.event.head_commit.message, 'skip fix')
            uses: peter-evans/create-pull-request@v4
            with:
              token: ${{ secrets.PAT || secrets.GITHUB_TOKEN }}
              commit-message: "[MegaLinter] Apply linters automatic fixes"
              title: "[MegaLinter] Apply linters automatic fixes"
              labels: bot
          - name: Create PR output
            if: steps.ml.outputs.has_updated_sources == 1 && (env.APPLY_FIXES_EVENT == 'all' || env.APPLY_FIXES_EVENT == github.event_name) && env.APPLY_FIXES_MODE == 'pull_request' && (github.event_name == 'push' || github.event.pull_request.head.repo.full_name == github.repository) && !contains(github.event.head_commit.message, 'skip fix')
            run: |
              echo "Pull Request Number - ${{ steps.cpr.outputs.pull-request-number }}"
              echo "Pull Request URL - ${{ steps.cpr.outputs.pull-request-url }}"

          # Push new commit if applicable (for now works only on PR from same repository, not from forks)
          - name: Prepare commit
            if: steps.ml.outputs.has_updated_sources == 1 && (env.APPLY_FIXES_EVENT == 'all' || env.APPLY_FIXES_EVENT == github.event_name) && env.APPLY_FIXES_MODE == 'commit' && github.ref != 'refs/heads/main' && (github.event_name == 'push' || github.event.pull_request.head.repo.full_name == github.repository) && !contains(github.event.head_commit.message, 'skip fix')
            run: sudo chown -Rc $UID .git/
          - name: Commit and push applied linter fixes
            if: steps.ml.outputs.has_updated_sources == 1 && (env.APPLY_FIXES_EVENT == 'all' || env.APPLY_FIXES_EVENT == github.event_name) && env.APPLY_FIXES_MODE == 'commit' && github.ref != 'refs/heads/main' && (github.event_name == 'push' || github.event.pull_request.head.repo.full_name == github.repository) && !contains(github.event.head_commit.message, 'skip fix')
            uses: stefanzweifel/git-auto-commit-action@v4
            with:
              branch: ${{ github.event.pull_request.head.ref || github.head_ref || github.ref }}
              commit_message: "[MegaLinter] Apply linters fixes"


          # Summary and status
          - run: echo "🎨 MegaLinter quality checks completed"
          - run: echo "🍏 Job status is ${{ job.status }}."
    ```

The MegaLinter Workflow uses a configuration file to define which linters should be run as well as specify linter specific configuration files.

!!! EXAMPLE "MegaLinter Linter configuration"
    ```yaml title=".github/config/megalinter.yaml"
    ---
    # Configuration file for MegaLinter
    #
    # General configuration:
    # https://oxsecurity.github.io/megalinter/configuration/
    #
    # Specfic Linters:
    # https://oxsecurity.github.io/megalinter/latest/supported-linters/

    # ------------------------
    # Linters

    # Run linters in parallel
    PARALLEL: true

    # ENABLE specific linters, all other linters automatically disabled
    ENABLE:
      - CLOJURE
      - CREDENTIALS
      - DOCKERFILE
      - MAKEFILE
      - MARKDOWN
      - GIT
      - SPELL
      - YAML
      - REPOSITORY


    # Linter specific configuration

    CLOJURE_CLJ_KONDO_CONFIG_FILE: ".github/config/clj-kondo-ci-config.edn"
    CLOJURE_CLJ_KONDO_ARGUMENTS: "--lint deps.edn"
    # CLOJURE_CLJ_KONDO_FILTER_REGEX_EXCLUDE: "dev|develop"
    CLOJURE_CLJ_KONDO_FILTER_REGEX_EXCLUDE: "resources"

    # CREDENTIALS_SECRETLINT_DISABLE_ERRORS: true
    CREDENTIALS_SECRETLINT_CONFIG_FILE: ".github/config/secretlintrc.json"

    MARKDOWN_MARKDOWNLINT_CONFIG_FILE: ".github/config/markdown-lint.jsonc"
    # MARKDOWN_MARKDOWNLINT_DISABLE_ERRORS: false
    MARKDOWN_MARKDOWN_LINK_CHECK_CONFIG_FILE: ".github/config/markdown-link-check.json"
    # MARKDOWN_MARKDOWN_LINK_CHECK_CLI_LINT_MODE: "project"
    # MARKDOWN_MARKDOWN_LINK_CHECK_DISABLE_ERRORS: false
    MARKDOWN_REMARK_LINT_DISABLE_ERRORS: true
    # MARKDOWN_MARKDOWN_TABLE_FORMATTER_DISABLE_ERRORS: false

    SPELL_CSPELL_DISABLE_ERRORS: true
    SPELL_MISSPELL_DISABLE_ERRORS: true

    # YAML_PRETTIER_FILTER_REGEX_EXCLUDE: (docs/)
    # YAML_YAMLLINT_FILTER_REGEX_EXCLUDE: (docs/)

    # Explicitly disable linters to ensure they are never run
    # DISABLE:
    #   - COPYPASTE # checks for excessive copy-pastes
    #   - SPELL # spell checking - often creates many false positives
    #   - CSS #

    # Disable linter features
    # DISABLE_LINTERS:
    #   - REPOSITORY_DEVSKIM # unnecessary URL TLS checks
    #   - REPOSITORY_CHECKOV # fails on root user in Dockerfile
    #   - REPOSITORY_SECRETLINT

    # Ignore all errors and return without error status
    # DISABLE_ERRORS: true

    # ------------------------

    # ------------------------
    # Fix Errors

    # Automatically update files with corrections
    APPLY_FIXES: all # all, none, or list of linter keys
    # ------------------------

    # ------------------------
    # Reporting

    # Activate sources reporter
    UPDATED_SOURCES_REPORTER: false

    # Show Linter timings in summary table at end of run
    SHOW_ELAPSED_TIME: true

    # Upload reports to file.io
    FILEIO_REPORTER: false
    # ------------------------

    # ------------------------
    # Over-ride errors

    # detect errors but do not block CI passing
    # DISABLE_ERRORS: true
    # ------------------------
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
          - uses: actions/checkout@v3
            with:
              fetch-depth: 0
          - uses: actions/setup-python@v4
            with:
              python-version: 3.x
          - uses: actions/cache@v3
            with:
              key: ${{ github.ref }}
              path: .cache
          - run: pip install mkdocs-material mkdocs-callouts mkdocs-glightbox mkdocs-git-revision-date-localized-plugin mkdocs-redirects pillow cairosvg
          - run: mkdocs gh-deploy --force
    ```

## Scheduled Version Check

Use [liquidz/antq-action](https://github.com/liquidz/antq-action) to check for new versions of Clojure libraries and GitHub action.

The GtiHub action can use the following actions

- `excludes:` list of space separated artefact names to exclude from the version check, use `groupId/artifactId` for Java libraries
- `directories:` search paths to check, space separated.
- `skips:` project types to skip to search, space separated. One of boot, clojure-cli, github-action, pom, shadow-cljs or leiningen.

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

          - name: "Version Check"
            uses: liquidz/antq-action@main
            # - excludes: "qualifier/libary-name groupId/artifactId"
            # - directories: "search/path/1 search/path/2"
            # - skips: "boot clojure-cli github-action pom shadow-cljs leiningen"

          - run: echo "🎨 Clojure library and GitHub Action versions checked with liquidz/antq"

          - run: echo "🍏 Job status is ${{ job.status }}."
    ```
