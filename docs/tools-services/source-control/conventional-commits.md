# Conventional Commits

A specification and convention for structuring version control commit messages to convey meaning for both people and tools alike.

Provides an easy to use form that supports creating an explicit change history, as well as enabling tool automation for that history.

Using [a consistent set of change types](#change-types) flattens the learning curve when contributing to the project.

The Conventional Commits specification includes the following elements:

- Type: Indicates the nature of the change (e.g., feat, fix, docs).
- Scope (optional): Specifies the area of the codebase affected.
- Description: A brief summary of the change.
- Body (optional): Additional details about the change.
- Footer (optional): References to issues or breaking changes.

General form of a conventional commit message

```shell-output
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

Breaking changes are marked with `!` after the type/scope, or including `BREAKING CHANGE: <description>` as the last line (footer) of the commit message body.

[Conventional Commits web page](https://www.conventionalcommits.org/){target=_blank .md-button}


## Change Types

- `build` changes affecting the build system or external dependencies
- `chore`
- `ci` continuous integration change, e.g. Git workflows and pipelines
- `docs` add or update documentation to the project
- `feat` relates to a feature of the project
- `fix` resolves a specific bug or issue in the source code
- `perf` performance related change
- `refactor` change to the design, architecture, language, library or tooling
- `style` changes not affecting the meaning of the code (blank-space, formatting, etc)
- `revert`  reverts a previous commit. Use the description from the reverted commit, the body it should state: `This reverts commit <hash>`, where hash is the SHA of the commit being reverted.
- `test` testing the project, e.g. unit & integration testing


A project could introduce more types, if there is significant value and their purpose is documented in the projects contribution guide.


## Body

The body should include the motivation for the change and contrast this with previous behavior.

Use the imperative, present tense: "change" not "changed" nor "changes".


## Footer

The footer should contain any information about related issue tickets or breaking changes.

Reference an issue using either `Related` or `Resolve`, followed by a reference to the issue, e.g. `resolve: #42`

`BREAKING CHANGE: ` when a commit introduces a breaking API change or any impactful change to users of the project.

Breaking Changes should start with the word BREAKING CHANGE: with a space or two newlines. The rest of the commit message is then used for this.

Other footers can follow a convention similar to [git trailer key/value format](https://git-scm.com/docs/git-interpret-trailers){target=_blank}.

??? EXAMPLE "Include footer tags in a commit message template"
    ```markdown
    # resolve: #
    # related: #
    # BREAKING CHANGE: <brief description>
    ```

## Tool support

- Neovim: [conventional-commits.nvim](https://github.com/zerbiniandrea/conventional-commits.nvim){target=_blank .md-button}


### Git Hook

??? EXAMPLE "Pre-receive hook for conventional commits"
    ```shell title=".git/hooks/pre-receive"
    #!/usr/bin/env bash

    # Pre-receive hook that will block commits with messages that do not follow regex rule

    commit_msg_type_regex='feat|fix|refactor|style|test|docs|build'
    commit_msg_scope_regex='.{1,20}'
    commit_msg_description_regex='.{1,100}'
    commit_msg_regex="^(${commit_msg_type_regex})(\(${commit_msg_scope_regex}\))?: (${commit_msg_description_regex})\$"
    merge_msg_regex="^Merge branch '.+'\$"

    zero_commit="0000000000000000000000000000000000000000"

    # Do not traverse over commits that are already in the repository
    excludeExisting="--not --all"

    error=""
    while read oldrev newrev refname; do
      # branch or tag get deleted
      if [ "$newrev" = "$zero_commit" ]; then
        continue
      fi

      # Check for new branch or tag
      if [ "$oldrev" = "$zero_commit" ]; then
        rev_span=`git rev-list $newrev $excludeExisting`
      else
        rev_span=`git rev-list $oldrev..$newrev $excludeExisting`
      fi

      for commit in $rev_span; do
        commit_msg_header=$(git show -s --format=%s $commit)
        if ! [[ "$commit_msg_header" =~ (${commit_msg_regex})|(${merge_msg_regex}) ]]; then
          echo "$commit" >&2
          echo "ERROR: Invalid commit message format" >&2
          echo "$commit_msg_header" >&2
          error="true"
        fi
      done
    done

    if [ -n "$error" ]; then
      exit 1
    fi
    ```

## Gitmojis

Visual indicators make browsing the commit history easier for a human to filter.

Commonly used Gitmojis from Conventional-Commits.nvim

New / Updates:

- 🎉 `:tada:` Begin a project.
- ✨ `:sparkles:` Introduce new features.
- 🚩 `:triangular_flag_on_post:` Add, update, or remove feature flags.
- 💄 `:lipstick:` Add or update the UI and style files.
- 🛂 `:passport_control:` Work on code related to authorization, roles and permissions.
- ⬆️ `:arrow_up:` Upgrade dependencies.
- ➕ `:heavy_plus_sign:` Add a dependency.
- 🔧 `:wrench:` Add or update configuration files.
- 🔨 `:hammer:` Add or update development scripts.
- 🚀 `:rocket:` Deploy stuff.
- 🔖 `:bookmark:` Release / Version tags.
- ✅ `:white_check_mark:` Add, update, or pass tests.
- 🔒️ `:lock:` Fix security or privacy issues.
- ♻️ `:recycle:` Refactor code.
- ⚰️ `:coffin:` Remove dead code.
- 🙈 `:see_no_evil:` Add or update a .gitignore file.

Corrections:

- 🔥 `:fire:` Remove code or files.
- 🐛 `:bug:` Fix a bug.
- 🚑️ `:ambulance:` Critical hotfix.

Docs:

- 📝 `:memo:` Add or update documentation.
- ✏️ `:pencil2:` Fix typos.


??? INFO "Gitmojis for Conventional Commits"
    [conventional-commits.nvim Gitmojis.json](https://github.com/zerbiniandrea/conventional-commits.nvim/blob/main/gitmojis.json){target=_blank}

    - 🎨 `:art:` I`mprove structure / format of the code.
    - ⚡️ `:zap:` Improve performance.
    - 🔥 `:fire:` Remove code or files.
    - 🐛 `:bug:` Fix a bug.
    - 🚑️ `:ambulance:` Critical hotfix.
    - ✨ `:sparkles:` Introduce new features.
    - 📝 `:memo:` Add or update documentation.
    - 🚀 `:rocket:` Deploy stuff.
    - 💄 `:lipstick:` Add or update the UI and style files.
    - 🎉 `:tada:` Begin a project.
    - ✅ `:white_check_mark:` Add, update, or pass tests.
    - 🔒️ `:lock:` Fix security or privacy issues.
    - 🔐 `:closed_lock_with_key:` Add or update secrets.
    - 🔖 `:bookmark:` Release / Version tags.
    - 🚨 `:rotating_light:` Fix compiler / linter warnings.
    - 🚧 `:construction:` Work in progress.
    - 💚 `:green_heart:` Fix CI Build.
    - ⬇️ `:arrow_down:` Downgrade dependencies.
    - ⬆️ `:arrow_up:` Upgrade dependencies.
    - 📌 `:pushpin:` Pin dependencies to specific versions.
    - 👷 `:construction_worker:` Add or update CI build system.
    - 📈 `:chart_with_upwards_trend:` Add or update analytics or track code.
    - ♻️ `:recycle:` Refactor code.
    - ➕ `:heavy_plus_sign:` Add a dependency.
    - ➖ `:heavy_minus_sign:` Remove a dependency.
    - 🔧 `:wrench:` Add or update configuration files.
    - 🔨 `:hammer:` Add or update development scripts.
    - 🌐 `:globe_with_meridians:` Internationalization and localization.
    - ✏️ `:pencil2:` Fix typos.
    - 💩 `:poop:` Write bad code that needs to be improved.
    - ⏪️ `:rewind:` Revert changes.
    - 🔀 `:twisted_rightwards_arrows:` Merge branches.
    - 📦️ `:package:` Add or update compiled files or packages.
    - 👽️ `:alien:` Update code due to external API changes.
    - 🚚 `:truck:` Move or rename resources (e.g.: files, paths, routes).
    - 📄 `:page_facing_up:` Add or update license.
    - 💥 `:boom:` Introduce breaking changes.
    - 🍱 `:bento:` Add or update assets.
    - ♿️ `:wheelchair:` Improve accessibility.
    - 💡 `:bulb:` Add or update comments in source code.
    - 🍻 `:beers:` Write code drunkenly.
    - 💬 `:speech_balloon:` Add or update text and literals.
    - 🗃️ `:card_file_box:` Perform database related changes.
    - 🔊 `:loud_sound:` Add or update logs.
    - 🔇 `:mute:` Remove logs.
    - 👥 `:busts_in_silhouette:` Add or update contributor(s).
    - 🚸 `:children_crossing:` Improve user experience / usability.
    - 🏗️ `:building_construction:` Make architectural changes.
    - 📱 `:iphone:` Work on responsive design.
    - 🤡 `:clown_face:` Mock things.
    - 🥚 `:egg:` Add or update an easter egg.
    - 🙈 `:see_no_evil:` Add or update a .gitignore file.
    - 📸 `:camera_flash:` Add or update snapshots.
    - ⚗️ `:alembic:` Perform experiments.
    - 🔍️ `:mag:` Improve SEO.
    - 🏷️ `:label:` Add or update types.
    - 🌱 `:seedling:` Add or update seed files.
    - 🚩 `:triangular_flag_on_post:` Add, update, or remove feature flags.
    - 🥅 `:goal_net:` Catch errors.
    - 💫 `:dizzy:` Add or update animations and transitions.
    - 🗑️ `:wastebasket:` Deprecate code that needs to be cleaned up.
    - 🛂 `:passport_control:` Work on code related to authorization, roles and permissions.
    - 🩹 `:adhesive_bandage:` Simple fix for a non-critical issue.
    - 🧐 `:monocle_face:` Data exploration/inspection.
    - ⚰️ `:coffin:` Remove dead code.
    - 🧪 `:test_tube:` Add a failing test.
    - 👔 `:necktie:` Add or update business logic.
    - 🩺 `:stethoscope:` Add or update healthcheck.
    - 🧱 `:bricks:` Infrastructure related changes.
    - 🧑‍💻 `:technologist:` Improve developer experience.
    - 💸 `:money_with_wings:` Add sponsorships or money related infrastructure.
    - 🧵 `:thread:` Add or update code related to multithreading or concurrency.
    - 🦺 `:safety_vest:` Add or update code related to validation.
    - ✈️ `:airplane:` Improve offline support.
    - 🦖 `:t-rex:` Code that adds backwards compatibility.
