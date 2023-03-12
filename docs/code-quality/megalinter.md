# MegaLinter

MegaLinter will verify code and configuration with a a wide range of Lint and formatting tools, all from a Docker image that minimises the install requirements.

MegaLinter is an Open-Source tool for CI/CD workflows that analyses the consistency of your code, IAC, configuration, and scripts in your  repository sources, to ensure all your projects sources are clean and formatted whatever IDE/toolbox is used.

!!! INFO "Requirements"
    Node.js is required to install and run the `mega-linter-runner` tool. Docker (Docker Desktop) is required to run MegaLinter containers locally, optionally with

    MegaLinter run via a GitHub workflow has no requirements


## GitHub Workflow

Create a `.github/workflows/megalinter.yaml` configuration and commit to the source code repository.

The MegaLinter Installation : GitHub Action provides a working example

Edit the MegaLinter step in jobs: and ENABLE linters required and provide linter specific configuration


```
 # Linters to run (all other Linters automatically disabled)  ENABLE: CLOJURE,DOCKERFILE,MAKEFILE,MARKDOWN,YAML
 # Linter specific configuration
 CLOJURE_CLJ_KONDO_CONFIG_FILE: ".clj-kondo/config-ci.edn"  CLOJURE_CLJ_KONDO_FILTER_REGEX_EXCLUDE: "dev"
 MARKDOWN_MARKDOWN_LINK_CHECK_CONFIG_FILE: ".github/linters /markdown-link-check.json"
```


Comment the `APPLY_FIXES` lines in the top-level env: section to only report issues, rather than automatically commit changes to fix the issues.

```
env:
 # Apply linter fixes configuration
 # APPLY_FIXES: all # APPLY_FIXES must be defined as environment  variable
 # APPLY_FIXES_EVENT: pull_request # events that trigger fixes on a  commit or PR (pull_request, push, all)
 # APPLY_FIXES_MODE: pull_request # are fixes are directly committed  (commit) or posted in a PR (pull_request)
```


!!! HINT "Reference: MegaLinter Configuration"
    [MegaLinter Configuration](https://megalinter.io/latest/installation/#github-action) provides a detailed breakdown of the configuration options


## Local Configuration

Create a configuration for a specific project using the MegaLinter runner command in the root of the project directory

```
npx mega-linter-runner --install

```

`.mega-linter.yml` file is created at the root of the project. Review the file and ensure it has the Linters required See MegaLinter Configuration for a breakdown of the configuration


## Run Locally

Install the MegaLinter for use across any project

```shell
npm install mega-linter-runner -g
```

Run the MegaLinter runner (initial run will be slow as the MegaLinter docker image downloads and is cached)

```shell
mega-linter-runner
```


## Optional optimisation

Use a specific flavour of MegaLinter to reduce the size of the Docker image used (a flavor is a docker image configuration for a specific programming  language and its tooling)
MEGALINGER_CONFIG environment variable defines an alternative location for the configuration file, e.g. `.github/linters/megalinter.yml`

Add a lint target to the project Makefile and run make lint to call the `mega-linter-runner`, especially if using a different `--flavor` or using an alternative location for the configuration file

```
# Run MegaLinter with custom configuration
lint:
 $(info --------- MegaLinter Runner ---------)
 mega-linter-runner --env 'MEGALINTER_CONFIG=.github/linters /mega-linter.yml'
```

!!! HINT "Options for MegaLinter Runner"
    [MegaLinter Runner options](https://megalinter.io/latest/mega-linter-runner/)


## MegaLinter Configuration

The declarative MegaLinter configuration is relatively easy to learn. Typically more time is spent tweaking the features (tools) each Linter category provides
Each Linter has its own page listing the tools it provides and each tool has its own page listing the configuration options and links to the tools  homepage.

Fraud API service contains a mega-linter.yml configuration file for running DOCKERFILE, MAKEFILE, YAML, MARKDOWN and CLOJURE Linters

### Configuring Linters

`ENABLE` or `DISABLE` specific linter groups DOCKERFILE, MAKEFILE, etc. or specific tools within a linter category, e.g. DISABLE the devskim tool  in the Repository linter category, REPOSITORY_DEVSKIM
Enabling specific Linters will disable all other linters that are not in the list

```shell
# Linters
# ENABLE specific linters, all other linters automatically disabled
ENABLE:CLOJURE,DOCKERFILE,MAKEFILE,MARKDOWN,YAML
```

The list format seems to cause errors in GitHub actions workflow configuration, so use a comma separated list

Pass configuration to the linter tools. Most tools have their configuration files and the default location is defined on the specific page for that tool.

```shell
# Linter specific configuration
CLOJURE_CLJ_KONDO_CONFIG_FILE: ".clj-kondo/config-ci.edn"
CLOJURE_CLJ_KONDO_FILTER_REGEX_EXCLUDE: "dev"
MARKDOWN_MARKDOWN_LINK_CHECK_CONFIG_FILE: ".github/linters/markdown link-check.json"
```

Disable Linters and specific tools (features)

```shell
# Explicitly disable linters to ensure they are never run
DISABLE:
 - COPYPASTE # checks for excessive copy-pastes
 - SPELL # spell checking - often creates many false positives  - CSS
# Disable linter features
DISABLE_LINTERS:
 - REPOSITORY_DEVSKIM # unnecessary URL TLS checks
 - REPOSITORY_CHECKOV # fails on root user in Dockerfile
 - REPOSITORY_SECRETLINT
 ```
