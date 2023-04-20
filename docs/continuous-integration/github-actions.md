# GitHub Actions

[GitHub Actions](https://docs.github.com/en/actions) are pre-defined tasks that can be used within a GitHub Workflow, triggered by events such as committing to a branch or pull request.

[GitHub Actions Marketplace](https://github.com/marketplace?type=actions)  contains a wide range of actions that can be used to quickly create a workflow.

!!! HINT "Use major version only"
    Use the major version of a GitHub action within a GitHub workflows to minimise the maintenance of action versions in workflow configuration.  e.g. if the latest version is `v5.2.1`, then use version `v5` to use the latest version available within that major version.


## Authentication

GitHub actions have read access to repositories, allowing them to checkout code and configuration.  For more permissions the GitHub Action requires an authorisation token.

GitHub automatically creates a token that GitHub actions can use to make an authenticated API request, scoped to the current repository.

```yaml title="Use automatically created token"
      - uses: actions/action-name@v2
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}
```

For customised authentication or access to a different GitHub repository, create a developer token and save it as a [user or organisation secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets).



[GitHub - Automatic token authentication](https://docs.github.com/en/actions/security-guides/automatic-token-authentication){target=_blank .md-button}


# Common GitHub Actions

| Action                                                                               | Description                                                                                             |
|--------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [Checkout](https://github.com/marketplace/actions/checkout)                          | Checkout repository to enable workflow to access                                                        |
| [Cache](https://github.com/marketplace/actions/cache)                                | cache dependencies and build outputs to improve workflow execution time                                 |
| [Changelog Enforcer](https://github.com/marketplace/actions/changelog-enforcer)      | checks the CHANGELOG.md file has been updated for a pull request                                        |
| [MegaLinter](https://github.com/marketplace/actions/megalinter)                      | verify code and configuration consistency                                                               |
| [Setup reviewdog](https://github.com/marketplace/actions/setup-reviewdog)            | Setup reviewdog post review comments to pull requests, typically used with lint tools                   |
| [Clojure lint](https://github.com/marketplace/actions/clojure-lint-action)           | clj-kondo lint with reviewdog comments added inline to pull request                                     |
| [Setup java jdk](https://github.com/marketplace/actions/setup-java-jdk)              | setup up Java run-time environment (specify version, distribution, etc)                                 |
| [Setup clojure](https://github.com/marketplace/actions/setup-clojure)                | Setup Clojure environment and common Clojure tools for quality (clj-kondo, cljstyle, unit testing, etc) |
| [Setup go](https://github.com/marketplace/actions/setup-go-environment)              | sets up a Go language environment                                                                       |
| [Setup node](https://github.com/marketplace/actions/setup-node-js-environment)       | setup a version of the Node.js environment, caching npm/yarn /pnpm dependencies                         |
| [Setup python](https://github.com/marketplace/actions/setup-python)                  | Provides python or PyPy, optionally cache pip dependencies                                              |
| [GitHub Pages Deploy](https://github.com/marketplace/actions/deploy-to-github-pages) | deploy to GitHub pages, including cross-repository deployments                                          |


Actions to investigate

- [clj-watson](https://github.com/clj-holmes/clj-watson) - software composition analysis (SCA)
- [clj-homes](https://github.com/marketplace/actions/clj-holmes-clojure) - SAST (Static application security testing) tool
- [Clojure Dependency Update Action](https://github.com/marketplace/actions/clojure-dependency-update-action) - update project dependency versions for Clojure CLI, Shadow-cljs, Leiningen, Boot and Maven Pom.xml
- [Clojure-Autodoc Action](https://github.com/marketplace/actions/clojure-documentation-action) - generate HTML API documentation from a Clojure project
- [Clojars Release Action](https://github.com/marketplace/actions/publish-to-clojars)
