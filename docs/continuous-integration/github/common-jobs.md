# Common GitHub Workflow Jobs

Common job configuration snippets taken from GitHub workflows, showing options typically used with GitHub actions.

## Basic workflow information

Echo information regarding the triggering of the workflow, to help diagnose issues should they arise

!!! EXAMPLE "Git Checkout Action"
    ```yaml
    jobs:
      workflow:
        name: workflow-name
        runs-on: ubuntu-latest
        steps:
          - run: echo "🚀 Job automatically triggered by ${{ github.event_name }}"
          - run: echo "🐧 Job running on ${{ runner.os }} server"
          - run: echo "🐙 Using ${{ github.ref }} branch from ${{ github.repository }} repository"
    ```

    Add a summary of the workflow at the end of the configuration.

    ```yaml
          # Summary and status
          - run: echo "🎨 Workflow name completed"
          - run: echo "🍏 Job status is ${{ job.status }}."
    ```


## Git Checkout

Check-out the project from version control, fetching the whole history with `fetch-depth: 0`. Set `fetch-depth:` to 1 (or remove the option) to checkout the single commit for the ref/SHA that triggered the workflow

Echo the GitHub Repository that was cloned to the workflow log, to support debugging efforts.

!!! EXAMPLE "Git Checkout Action"
    ```yaml
          # Git Checkout
          - name: Checkout Code
            uses: actions/checkout@v3
            with:
              token: "${{ secrets.PAT || secrets.GITHUB_TOKEN }}"
              fetch-depth: 0   # fetch all history
          - run: echo "🐙 ${{ github.repository }} repository was cloned to the runner."
    ```


## Pull Request first interaction

!!! WARNING "first-interaction returning Bad Credentials error"

Add messages to a contributor's first issue or pull request to a repository.

In this example, the contributing guide is added as a comment to the issue or pull request.

!!! EXAMPLE "First Interaction Action"
    ```yaml
          # Message on first interaction
          - name: First interaction
            uses: actions/first-interaction@v1.1.1
            with:
              # Token for the repository
              repo-token: "{{ secrets.GITHUB_TOKEN }}"
              # Comment to post on an individual's first issue
              issue-message: "[Practicalli Contributing Guide](https://practical.li/spacemacs/introduction/contributing/)"
              # Comment to post on an individual's first pull request
              pr-message: "[Practicalli Contributing Guide](https://practical.li/spacemacs/introduction/contributing/)"
    ```
