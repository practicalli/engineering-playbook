# MegaLinter

[:globe_with_meridians: MegaLinter](https://megalinter.io/) verifies code and configuration with a a wide range of Lint and formatting tools, all from within a Docker image that minimises the install requirements.

MegaLinter is an Open-Source tool for CI/CD workflows that analyses the consistency of your code, IAC, configuration, and scripts in your  repository sources, to ensure all your projects sources are clean and formatted whatever IDE/toolbox is used.

The declarative MegaLinter configuration is relatively easy to learn. Typically time is spent tweaking the features (tools) of the [:globe_with_meridians: supported Linters](https://megalinter.io/latest/supported-linters/){target=_blank}

Supported Linters have their own page listing the lint and format tools it provides, e.g. [:globe_with_meridians: Clojure](https://megalinter.io/latest/descriptors/clojure/). Each tool has its own page listing the configuration options and links to the tool homepage, e.g. [:globe_with_meridians: clj-kondo](https://megalinter.io/latest/descriptors/clojure_clj_kondo/).

!!! INFO "Requirements"
    [:globe_with_meridians: Node.js](https://nodejs.org/){target=_blank} is required to install and run the `mega-linter-runner` tool. [Docker (Docker Desktop)](/engineering-playbook/continuous-integration/docker/) is required to run MegaLinter containers locally, optionally with

    MegaLinter run via a GitHub workflow has no requirements

## MegaLinter Locally

The most effective way to manage lint and format issues is to run MegaLinter locally, before pushing changes to a Continuous Integration service.

The MegaLinter runner uses a MegaLinter docker image will all lint tools pre-installed and configured, minimising the need to install tools locally.

Install a recent version of [:globe_with_meridians: Node.js](https://nodejs.org/){target=_blank}, version 18 (Long Term Support) is recommended.

> `npx mega-linter-runner` runs MegaLinter without the need for a specific npm install.

### Create a configuration

!!! HINT "Use the Practicalli MegaLinter Configuration"
    [Practicalli MegaLinter configuration](#megalinter-configuration) configures tools relevant to Clojure development. Practicalli Project Templates also include Megalinter configuration and GitHub workflow.

Create a configuration for a specific project using the MegaLinter runner command in the root of the project directory

```
npx mega-linter-runner
```

`.mega-linter.yml` file is created at the root of the project. Review the file and ensure it has the Linters required. [:globe_with_meridians: MegaLinter Configuration](https://megalinter.io/latest/configuration/) describes the purpose of each variable.

### Run MegaLinter

Use a specific flavour of MegaLinter to reduce the size of the Docker image used. A flavor is a docker image configuration for a specific programming language and its tooling.

- `--flavor java` for Clojure projects
- `--flavor documentation` for markdown / documentation only projects

Other useful options include

- `--env "'MEGALINTER_CONFIG=.github/config/megalinter.yaml'"` to pass a specific MegaLinter configuration file
- `--remove-container` to remove the docker container once MegaLinter run has completed (the MegaLinter image will still be available)

Run the MegaLinter runner with these options

!!! NOTE ""
    ```shell
    npx mega-linter-runner --flavor java --env "'MEGALINTER_CONFIG=.github/config/megalinter.yaml'" --remove-container
    ```

The initial run will be slow as the MegaLinter docker image downloads and its overlays are cached.  All following runs will be significantly faster.

!!! HINT "make lint"
    `make lint` will run the MegaLinter runner using the above options, using the `lint` task from [Practicalli Makefile](/engineering-playbook/build-tool/make/)

### Automatically fix issues

Use the `--fix` option with the local MegaLinter runner to automatically apply lint and format fixes that MegaLinter discovers.

??? HINT "Stage or Commit changes before MegaLinter runner automatic fix"
    If code and configuration changes are staged or committed before running with `--fix` then automatic fixes can easily be discarded or treated as a separate commit using any Git tool.

!!! NOTE ""
    ```shell
    npx mega-linter-runner --fix --flavor java --env "'MEGALINTER_CONFIG=.github/config/megalinter.yaml'" --remove-container
    ```

## Make tasks

Add a `lint` task to the project Makefile and run `make lint` to call the `mega-linter-runner`. Especially useful when using a different `--flavor` or using an alternative location for the configuration file, so the same approach is used each time.

!!! EXAMPLE "Makefile tasks for MegaLinter Runner"
    ```make
    lint:
     $(info --------- MegaLinter Runner ---------)
        npx mega-linter-runner --flavor java --env "'MEGALINTER_CONFIG=.github/config/megalinter.yaml'" --remove-container

    lint-fix:
     $(info --------- MegaLinter Runner ---------)
        npx mega-linter-runner --fix --flavor java --env "'MEGALINTER_CONFIG=.github/config/megalinter.yaml'" --remove-container
    ```

!!! HINT "Options for MegaLinter Runner"
    [MegaLinter Runner options](https://megalinter.io/latest/mega-linter-runner/)

## MegaLinter Configuration

Create a configuration file to use with the local `mega-linter-runner` and [GitHub workflow](#github-workflow)

`.github/config/megalinter.yaml` is a recommeded location for the MegaLinter configuration file, especially when using GitHub workflow.

??? EXAMPLE "Practicalli MegaLinter Configuration"
    ```yaml
    ---
    # Configuration file for MegaLinter
    #
    # General configuration:
    # <https://oxsecurity.github.io/megalinter/configuration/>
    #
    # Specific Linters:
    # <https://oxsecurity.github.io/megalinter/latest/supported-linters/>

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
    # CLOJURE_CLJ_KONDO_ARGUMENTS: "--lint deps.edn"
    # CLOJURE_CLJ_KONDO_FILTER_REGEX_EXCLUDE: "dev|develop"
    CLOJURE_CLJ_KONDO_FILTER_REGEX_EXCLUDE: "resources"

    # CREDENTIALS_SECRETLINT_DISABLE_ERRORS: true
    CREDENTIALS_SECRETLINT_CONFIG_FILE: ".github/config/secretlintrc.json"

    MARKDOWN_MARKDOWNLINT_CONFIG_FILE: ".github/config/markdown-lint.jsonc"
    MARKDOWN_MARKDOWNLINT_FILTER_REGEX_EXCLUDE: ".github/pull_request_template.md|CHANGELOG.md"
    # MARKDOWN_MARKDOWNLINT_DISABLE_ERRORS: false
    MARKDOWN_MARKDOWN_LINK_CHECK_CONFIG_FILE: ".github/config/markdown-link-check.json"
    # MARKDOWN_MARKDOWN_LINK_CHECK_CLI_LINT_MODE: "project"
    # MARKDOWN_MARKDOWN_LINK_CHECK_DISABLE_ERRORS: false
    MARKDOWN_REMARK_LINT_DISABLE_ERRORS: true
    # MARKDOWN_MARKDOWN_TABLE_FORMATTER_DISABLE_ERRORS: false

    # SPELL_CSPELL_DISABLE_ERRORS: true
    SPELL_MISSPELL_DISABLE_ERRORS: true

    # YAML_PRETTIER_FILTER_REGEX_EXCLUDE: (docs/)
    # YAML_YAMLLINT_FILTER_REGEX_EXCLUDE: (docs/)

    # Explicitly disable linters to ensure they are never run
    # DISABLE:
    #   - COPYPASTE # checks for excessive copy-pastes
    #   - SPELL # spell checking - often creates many false positives
    #   - CSS #

    # Disable linter features
    DISABLE_LINTERS:
      - YAML_PRETTIER # draconian format rules
      - SPELL_CSPELL # many clojure references causing false positives
      - YAML_YAMLLINT # vague error mesages, investigation required
      - REPOSITORY_GIT_DIFF # warnings about LF to CRLF
      - REPOSITORY_SECRETLINT # reporting errors in its own config file
      # - REPOSITORY_DEVSKIM # unnecessary URL TLS checks
      - REPOSITORY_CHECKOV # fails on root user in Dockerfile
      - REPOSITORY_SECRETLINT

    # Ignore all errors and return without error status
    # DISABLE_ERRORS: true

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

### Optomise Run

Run the linters in paralell to speed up the overall MegaLinter run

```YAML
PARALLEL: true
```

### Linter groups

Enable the linter groups to run. Specific linters within these groups can be enabled/disabled using Linter specific variables

```yaml
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
```

### Linter configuration

Configure the Clojure lint tools, which currently includes clj-kondo

- `CLOJURE_CLJ_KONDO_CONFIG_FILE` to define clj-kondo configuration and lint rules to use
- `CLOJURE_CLJ_KONDO_ARGUMENTS` pass arguments such as `--lint` to define a path to check rather than the whole project
- `CLOJURE_CLJ_KONDO_FILTER_REGEX_EXCLUDE` to exclude one or more files or directories

Example:

```yaml
CLOJURE_CLJ_KONDO_CONFIG_FILE: ".github/config/clj-kondo-ci-config.edn"
CLOJURE_CLJ_KONDO_ARGUMENTS: "--lint deps.edn"
CLOJURE_CLJ_KONDO_FILTER_REGEX_EXCLUDE: "dev|develop"
```

Explicitly disable linter tools to ensure they are never run

```yaml
DISABLE_LINTERS:
  - YAML_PRETTIER # draconian format rules
  - SPELL_CSPELL # many clojure references causing false positives
  - YAML_YAMLLINT # vague error mesages, investigation required
  - REPOSITORY_GIT_DIFF # warnings about LF to CRLF
  - REPOSITORY_SECRETLINT # reporting errors in its own config file
  # - REPOSITORY_DEVSKIM # unnecessary URL TLS checks
  - REPOSITORY_CHECKOV # fails on root user in Dockerfile
  - REPOSITORY_SECRETLINT
```

Disable all errors if there are configuration issues that need troubleshooting

```yaml
# DISABLE_ERRORS: true
```

### Reporting

Show Linter timings in summary table at end of run

```yaml
SHOW_ELAPSED_TIME: true
```

Upload reports to file.io

```yaml
FILEIO_REPORTER: false
```

## GitHub Workflow

[:fontawesome-solid-book-open: Practicalli MegaLinter GitHub Workflow](/engineering-playbook/continuous-integration/github/workflows/megalinter/)

Create a `.github/workflows/megalinter.yaml` workflow configuration, e.g [:fontawesome-solid-book-open: Practicalli MegaLinter Workflow](/engineering-playbook/continuous-integration/github/workflows/megalinter/)
