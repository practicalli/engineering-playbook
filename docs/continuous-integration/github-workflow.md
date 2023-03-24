# GitHub workflows

GitHub workflows used by Practicalli

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
    name: Changelog Check
    on:
      pull_request:
      paths-ignore:
        - "README.md"
        - "CHANGELOG.md"
      types: [opened, synchronize, reopened, ready_for_review, labeled, unlabeled]

    jobs:
      # Check CHANGELOG.md file updated on every pull request
      changelog:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3
          - uses: dangoslen/changelog-enforcer@v2
        with:
          changeLogPath: "CHANGELOG.md"
          skipLabels: "changelog-check-skip"
    ```


## Clojure Lint with Reviewdog

!!! EXAMPLE "clj-kondo lint with reviewdog reports"
    ```yaml
    ---
    name: Lint Review
    on: [pull_request]
    jobs:
      clj-kondo:
        name: runner / clj-kondo
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3.0.2
          - name: clj-kondo
            uses: nnichols/clojure-lint-action@v2
            with:
              pattern: "*.clj"
              clj_kondo_config: ".clj-kondo/config-ci.edn"
              level: "error"
              exclude: ".cljstyle"
              github_token: ${{ secrets.github_token }}
              reporter: github-pr-review
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
          - name: "Checkout code"
            uses: actions/checkout@v3.0.2

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
            uses: DeLaGuardo/setup-clojure@7.0
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
    ```yaml
    ---
    # MegaLinter GitHub Action configuration file
    # https://oxsecurity.github.io/megalinter

    name: MegaLinter

    # Trigger mega-linter at every push. Action will also be visible from Pull Requests to main
    on:
      push:
      pull_request:
        branches: [main]

    env:
      # Apply linter fixes configuration
      APPLY_FIXES: all # APPLY_FIXES must be defined as environment variable
      APPLY_FIXES_EVENT: pull_request # events that trigger fixes on a commit or PR (pull_request, push, all)
      APPLY_FIXES_MODE: pull_request # are fixes are directly committed (commit) or posted in a PR (pull_request)

    # Run Linters in parallel
    concurrency:
      group: "${{ github.ref }}-${{ github.workflow }}"
      cancel-in-progress: true

    jobs:
      build:
        name: MegaLinter
        runs-on: ubuntu-latest
        steps:
          # Git Checkout
          - name: Checkout Code
            uses: actions/checkout@v3
            with:
              token: ${{ secrets.PAT || secrets.GITHUB_TOKEN }}

          # MegaLinter Configuration
          - name: MegaLinter Run
            id: ml
            ## latest release of major version
            uses: oxsecurity/megalinter/flavors/java@v6.12.0
            env:
              MEGALINTER_CONFIG: .github/linters/mega-linter.yml
              GITHUB_TOKEN: "${{ secrets.GITHUB_TOKEN }}" # report individual linter status
              # Validate all source when push on main, else just the git diff with live.
              VALIDATE_ALL_CODEBASE: >-
                ${{ github.event_name == 'push' && github.ref == 'refs/heads/main'}}

              # Run linters in parallel
              PARALLEL: true

              # Linters to run (all other Linters automatically disabled)
              ENABLE: CLOJURE,DOCKERFILE,MAKEFILE,MARKDOWN,YAML

              # Linter specific configuration
              CLOJURE_CLJ_KONDO_CONFIG_FILE: ".clj-kondo/config-ci.edn"
              CLOJURE_CLJ_KONDO_FILTER_REGEX_EXCLUDE: "dev|develop"
              MARKDOWN_MARKDOWN_LINK_CHECK_CONFIG_FILE: ".github/linters/markdown-link-check.json"
              YAML_PRETTIER_FILTER_REGEX_EXCLUDE: (docs/)
              YAML_YAMLLINT_FILTER_REGEX_EXCLUDE: (docs/)

              # Explicitly disable linters to ensure they are never run
              # DISABLE:

              # Disable linter features
              # DISABLE_LINTERS:

              # ADD CUSTOM ENV VARIABLES OR DEFINE IN A FILE .mega-linter.yml AT ROOT OF REPOSITORY
              # DISABLE: COPYPASTE,SPELL # Uncomment to disable copy-paste and spell checks

              # Complete without returning error status
              # DISABLE_ERRORS: true

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
