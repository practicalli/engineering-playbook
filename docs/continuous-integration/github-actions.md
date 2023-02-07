# GitHub Actions

[GitHub Actions](https://docs.github.com/en/actions) are pre-defined tasks that can be used within a GitHub Workflow, triggered by events such as committing to a branch or pull request.

[GitHub Actions Marketplace]()  contains a wide range of actions that can be used to quickly create a workflow.


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