# MegaLinter Workflow

The MegaLinter Workflow uses a configuration file to define which linters should be run as well as specify linter specific configuration files.

!!! EXAMPLE "Practicalli MegaLinter Workflow"

    ```clojure
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
              # ADD CUSTOM ENV VARIABLES OR DEFINE IN MEGALINTER_CONFIG file
              MEGALINTER_CONFIG: .github/config/megalinter.yaml

              GITHUB_TOKEN: "${{ secrets.GITHUB_TOKEN }}" # report individual linter status
              # Validate all source when push on main, else just the git diff with live.
              VALIDATE_ALL_CODEBASE: >-
                ${{ github.event_name == 'push' && github.ref == 'refs/heads/main'}}

          # Upload MegaLinter artifacts
          - name: Archive production artifacts
            if: ${{ success() }} || ${{ failure() }}
            uses: actions/upload-artifact@v3
            with:
              name: MegaLinter reports
              path: |
                megalinter-reports
                mega-linter.log

          # Summary and status
          - run: echo "🎨 MegaLinter quality checks completed"
          - run: echo "🍏 Job status is ${{ job.status }}."
    ```

## Apply Fixes

The MegaLinter workflow can also apply fixes it finds to pull requests and commit_message

!!! WARNING "Applying fixes can make for confusing commits"
    Using the `--fix` option with the [:fontawesome-solid-book-open: local MegaLinter runner](/engineering-playbook/code-quality/megalinter/#run-megalinter) is a more effective way to manage automatic fixes by MegaLinter, especially if code and configuration changes are staged or committed before automatically fixing.  Automatic fixes can then be discarded or treated as a separate commit using any Git tool.

??? EXAMPLE "MegaLinter Workflow with Apply Fixes"
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

!!! HINT "Reference: MegaLinter Configuration"
    [MegaLinter Installation](https://megalinter.io/latest/installation/#github-action) defines a GitHub workflow which includes creating a commit or pull request to automatically apply fixes.
