# MegaLinter Workflow

The MegaLinter Workflow uses a configuration file to define which linters should be run as well as specify linter specific configuration files.

MegaLinter provides flavors that contain a sub-set of linters. The Java Flavor is suitable for Clojure projects and the Documentation flavor is suitable for documentation sites like Zensical.

Practicalli create custom flavors optomised for Clojure and Docs project, above and beyond the MegaLinter flavors.


[Clojure custom flavor for MegaLinter](https://github.com/practicalli/megalinter-custom-flavor-clojure){target=_blank .md-button}

[Zensical custom flavor for MegaLinter](https://github.com/practicalli/megalinter-custom-flavor-zensical){target=_blank .md-button}

!!! EXAMPLE "Practicalli Clojure custom flavor of MegaLinter"
    ```yaml
    - name: MegaLinter
      uses: practicalli/megalinter-custom-flavor-clojure@main
    ```


!!! EXAMPLE "Practicalli Zensical custom flavor of MegaLinter"
    ```yaml
    - name: MegaLinter
      uses: practicalli/megalinter-custom-flavor-zensical@main
    ```


!!! EXAMPLE "MegaLinter for Docs using Practicalli Custom Flavor for Zensical"
    ```yaml
    # MegaLinter GitHub Action configuration file
    # More info at https://megalinter.io
    ---
    name: MegaLinter

    # Trigger mega-linter at every push. Action will also be visible from
    # Pull Requests to main
    on:
      # Comment this line to trigger action only on pull-requests
      # (not recommended if you don't pay for GH Actions)
      push:

      pull_request:
        branches:
          - main

    # Comment env block if you do not want to apply fixes
    env:
      # Apply linter fixes configuration
      #
      # When active, APPLY_FIXES must also be defined as environment variable
      # (in github/workflows/mega-linter.yml or other CI tool)
      APPLY_FIXES: all

      # Decide which event triggers application of fixes in a commit or a PR
      # (pull_request, push, all)
      APPLY_FIXES_EVENT: pull_request

      # If APPLY_FIXES is used, defines if the fixes are directly committed (commit)
      # or posted in a PR (pull_request)
      APPLY_FIXES_MODE: pull_request

      # ADD CUSTOM ENV VARIABLES OR DEFINE IN MEGALINTER_CONFIG file
      MEGALINTER_CONFIG: .github/config/mega-linter.yaml

    concurrency:
      group: ${{ github.ref }}-${{ github.workflow }}
      cancel-in-progress: true

    permissions: {}

    jobs:
      megalinter:
        name: MegaLinter
        runs-on: ubuntu-latest

        # Give the default GITHUB_TOKEN write permission to commit and push, comment
        # issues, and post new Pull Requests; remove the ones you do not need
        permissions:
          contents: write
          issues: write
          packages: read
          pull-requests: write

        steps:
          # Git Checkout
          - name: Checkout Code
            uses: actions/checkout@v7
            with:
              token: ${{ secrets.PAT || secrets.GITHUB_TOKEN }}
              # persist-credentials: false # Comment this line and uncomment the next one if you use APPLY_FIXES
              persist-credentials: true # zizmor: ignore[artipacked]

              # If you use VALIDATE_ALL_CODEBASE = true, you can remove this line to
              # improve performance
              fetch-depth: 0

          # MegaLinter
          - name: MegaLinter
            uses: practicalli/megalinter-custom-flavor-zensical@main
            id: ml

            # All available variables are described in documentation
            # https://megalinter.io/latest/config-file/
            env:
              VALIDATE_ALL_CODEBASE: true

              # Disable LLM Advisor for bot PRs (dependabot, renovate, etc.)
              LLM_ADVISOR_ENABLED: >-
                ${{
                  github.event_name != 'pull_request' ||
                  (github.event.pull_request.user.login != 'dependabot[bot]' &&
                   github.event.pull_request.user.login != 'renovate[bot]' &&
                   github.event.pull_request.user.login != 'github-actions[bot]' &&
                   !startsWith(github.event.pull_request.user.login, 'dependabot') &&
                   !startsWith(github.event.pull_request.user.login, 'renovate'))
                }}

              GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

              # Uncomment to use ApiReporter (Grafana)
              API_REPORTER: true
              API_REPORTER_URL: ${{ secrets.API_REPORTER_URL }}
              API_REPORTER_BASIC_AUTH_USERNAME: ${{ secrets.API_REPORTER_BASIC_AUTH_USERNAME }}
              API_REPORTER_BASIC_AUTH_PASSWORD: ${{ secrets.API_REPORTER_BASIC_AUTH_PASSWORD }}
              API_REPORTER_METRICS_URL: ${{ secrets.API_REPORTER_METRICS_URL }}
              API_REPORTER_METRICS_BASIC_AUTH_USERNAME: ${{ secrets.API_REPORTER_METRICS_BASIC_AUTH_USERNAME }}
              API_REPORTER_METRICS_BASIC_AUTH_PASSWORD: ${{ secrets.API_REPORTER_METRICS_BASIC_AUTH_PASSWORD }}
              API_REPORTER_DEBUG: false

              # ADD YOUR CUSTOM ENV VARIABLES HERE TO OVERRIDE VALUES OF
              # .mega-linter.yml AT THE ROOT OF YOUR REPOSITORY

          # Upload MegaLinter artifacts
          - name: Archive production artifacts
            uses: actions/upload-artifact@v7
            if: success() || failure()
            with:
              name: MegaLinter reports
              include-hidden-files: "true"
              path: |
                megalinter-reports
                mega-linter.log

          # Create pull request if applicable
          # (for now works only on PR from same repository, not from forks)
          - name: Create Pull Request with applied fixes
            uses: peter-evans/create-pull-request@v8
            id: cpr
            if: >-
              steps.ml.outputs.has_updated_sources == 1 &&
              (
                env.APPLY_FIXES_EVENT == 'all' ||
                env.APPLY_FIXES_EVENT == github.event_name
              ) &&
              env.APPLY_FIXES_MODE == 'pull_request' &&
              (
                github.event_name == 'push' ||
                github.event.pull_request.head.repo.full_name == github.repository
              ) &&
              !contains(github.event.head_commit.message, 'skip fix')
            with:
              # SECURITY NOTE: see the warning on the checkout step above —
              # using `secrets.PAT` is NOT recommended for security reasons.
              # Prefer manually re-running the workflow or pushing another
              # commit on the branch to trigger workflows again.
              token: ${{ secrets.PAT || secrets.GITHUB_TOKEN }}
              commit-message: "[MegaLinter] Apply linters automatic fixes"
              title: "[MegaLinter] Apply linters automatic fixes"
              labels: bot

          - name: Create PR output
            if: >-
              steps.ml.outputs.has_updated_sources == 1 &&
              (
                env.APPLY_FIXES_EVENT == 'all' ||
                env.APPLY_FIXES_EVENT == github.event_name
              ) &&
              env.APPLY_FIXES_MODE == 'pull_request' &&
              (
                github.event_name == 'push' ||
                github.event.pull_request.head.repo.full_name == github.repository
              ) &&
              !contains(github.event.head_commit.message, 'skip fix')
            env:
              PR_NUMBER: ${{ steps.cpr.outputs.pull-request-number }}
              PR_URL: ${{ steps.cpr.outputs.pull-request-url }}
            run: |
              echo "PR Number - ${PR_NUMBER}"
              echo "PR URL - ${PR_URL}"

          # Push new commit if applicable
          # (for now works only on PR from same repository, not from forks)
          - name: Prepare commit
            if: >-
              steps.ml.outputs.has_updated_sources == 1 &&
              (
                env.APPLY_FIXES_EVENT == 'all' ||
                env.APPLY_FIXES_EVENT == github.event_name
              ) &&
              env.APPLY_FIXES_MODE == 'commit' &&
              github.ref != 'refs/heads/main' &&
              (
                github.event_name == 'push' ||
                github.event.pull_request.head.repo.full_name == github.repository
              ) &&
              !contains(github.event.head_commit.message, 'skip fix')
            run: sudo chown -Rc $UID .git/

          - name: Commit and push applied linter fixes
            uses: stefanzweifel/git-auto-commit-action@v7
            if: >-
              steps.ml.outputs.has_updated_sources == 1 &&
              (
                env.APPLY_FIXES_EVENT == 'all' ||
                env.APPLY_FIXES_EVENT == github.event_name
              ) &&
              env.APPLY_FIXES_MODE == 'commit' &&
              github.ref != 'refs/heads/main' &&
              (
                github.event_name == 'push' ||
                github.event.pull_request.head.repo.full_name == github.repository
              ) &&
              !contains(github.event.head_commit.message, 'skip fix')
            with:
              branch: >-
                ${{
                  github.event.pull_request.head.ref ||
                  github.head_ref ||
                  github.ref
                }}
              commit_message: "[MegaLinter] Apply linters fixes"
              commit_user_name: megalinter-bot
              commit_user_email: 129584137+megalinter-bot@users.noreply.github.com
    ```




??? EXAMPLE "Practicalli MegaLinter Workflow using Java flavor"

    ```clojure
    ---
    # MegaLinter GitHub Action configuration file
    # More info at https://megalinter.io
    # All variables described in https://megalinter.io/latest/config-file/

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
            uses: actions/checkout@v7
            with:
              token: "${{ secrets.PAT || secrets.GITHUB_TOKEN }}"
              fetch-depth: 0
              sparse-checkout: |
                docs
                overrides
                .github
          - run: echo "🐙 Sparse Checkout of ${{ github.repository }} repository to the CI runner."

          # MegaLinter Configuration
          - name: MegaLinter Run
            id: ml
            ## latest release of major version
            uses: oxsecurity/megalinter/flavors/java@v10
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
            uses: actions/upload-artifact@v7
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
