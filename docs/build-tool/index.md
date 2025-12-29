# Build Tools

Build tools can support a wide range of development tasks

* [GNU make](make.md) - language agnostic build tool, define any tasks
* [babashka task runner](babashka-task-runner.md) - write a build tool using Clojure syntax

## Common task naming

Tasks used across Practicalli projects follow the [make standard targets for users](https://www.gnu.org/software/make/manual/html_node/Standard-Targets.html)

> `all` , `test-ci` , `deps` and `dist` targets are recommended for use with a CI deployment pipeline and builder stage when using Docker.

- `all`  calling all targets to prepare the application to be run. e.g. `all: deps test-ci dist clean`
- `deps` download library dependencies (depend on `deps.edn` file)
- `dist` create a distribution tar file for this program or zip deployment package for AWS Lambda
- `lint` run lint tools to check code quality  - e.g [MegaLinter](https://oxsecurity.github.io/megalinter/) which provides a wide range of tools
- `format-check` report format and style issues for a specific programming language
- `format-fix` update source code files if there are format and style issues for a specific programming language
- `pre-commit` run unit tests and code quality targets before considering a Git commit
- `repl` run an interactive run-time environment for the programming language
- `test-unit` run all unit tests
- `test-ci` test running in CI build (optionally focus on integration testing)
- `clean` remove files created by any of the commands from other targets (i.e. ensure a clean build each time)

[:fontawesome-brands-github: practicalli/dotfiles/Makefile](https://github.com/practicalli/dotfiles/blob/main/Makefile) also defines docker targets to build and compose images locally, inspect images and prune containers and images.
