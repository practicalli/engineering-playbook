# Git Client Configuration

Git uses either `XDG_CONFIG_HOME/git/config` or `$HOME/.gitconfig` configuration file for user level settings for the Git client.

Editor Git support should use the Git client configuration.

??? HINT "Practicalli Dotfiles Git Configuration"
    [:fontawesome-brands-github: Practicalli Dotfiles](https://github.com/practicalli/dotfiles) contains an example Git user configuration, with separate identity configuration files for commercial and open source work.

    The Git configuration also provides global Git ignore patterns for Clojure and MkDocs projects.

    [practicalli/dotfiles Git config files](https://github.com/practicalli/dotfiles){target=_blank .md-button}


## Git identity

An identity is required when sharing commits via services such as GitHub/GitLab and so that each commit you make is associated to you.

Define your git identity using the following commands in a terminal window

??? HINT "Use the GitHub Email Mask address"
    To minimise Email spam, use the [email address provided by GitHub as a mask to your primary email address](https://github.com/settings/emails) on the GitHub account.  The mask address is of the form `***+github-account@noreply.github.com`.

    [Visit the email settings of the GitHub account](https://github.com/settings/emails) and tick **Keep my email addresses private**.

    A new email of the form `******+github-account-name@users.noreply.github.com` is created which must be set as your user email address

    For additional security, select the option **Block command line pushes that expose my email** to prevent commits being pushed to GitHub using your public email address.

```shell
git config --global user.name "practicalli-johnny"

git config --global user.email ***+github-account@noreply.github.com
```

The `[user]` section of the Git configuration file is updated by these commands, automatically creating the file and section if it does not exist.

??? EXAMPLE "Git Configuration"
    ```shell title=".config/git/identity-clojure-inc"
    # Add identity to all commits (required for GitHub / GitLab)
    [user]
     name = Practicalli Johnny

        # add email to Personal GitHub account via Settings > Email
     email = "johnny@clojure.inc"

    ## Identity for using GitHub API
    [github]
     user = practicalli-johnny

    ## SSH Keys - add key passphrase to MacOSX key chain
    [credential]
        helper = osxkeychain
    ```

### Multiple Git Identities

When working on a mixture of commercial and Open Source projects, configure the Git client with multiple identities

!!! EXAMPLE "Git Clone alias"
    ```shell title=".config/git/identity-clojure-inc"
    ## ------ Git Config: Identity ------ ##

    # Default identity configuration
    [include]
      path = ~/.config/git/identity-practicalli-johnny

    # Override identify for specific directories
    [includeIf "gitdir:~/projects/identity-clojure-inc"]
      path = ~/.config/git/identity-clojure-inc
    ```

??? INFO "MacOSX Path expansion not working"
    MacOSX did not expand ~ or $HOME for relative paths for identity files when using the latest MacOSX version and Git client from homebrew.

Included configure file with company identity.

??? EXAMPLE "Git Clone alias"
    ```shell title=".config/git/identity-clojure-inc"
    ## ------ Company Identity ------ ##
    # Add details for specific company identity

    # Add identity to all commits (required for GitHub / GitLab)
    [user]
     name = Practicalli Johnny

        # add email to Personal GitHub account via Settings > Email
     email = "johnny@clojure.inc"

    ## Identity for using GitHub API
    [github]
     user = practicalli-johnny

    ## SSH Keys - add key passphrase to MacOSX key chain
    [credential]
        helper = osxkeychain
    ```

## SSH Keys

SSH Keys provide a secure way to authorise requests to a shared service and verify the author of commits sent to a shared service.

!!! NOTE "Generate an SSH Key pair with Git email identity"
    Generate a public & private key pair using the recommended `ed25519` format.  Include the email address added to your GitHub account, or use the email mask provides by GitHub to avoid exposing your personal email.

    Enter a passphrase when prompted.  A 12 character or greater passphrase should provide adequate security.

    ```shell
    ssh-keygen -t ed25519 -C "******+practicalli-johnny@users.noreply.github.com"
    ```

??? HINT "Use the GitHub Email Mask address"
    Minimise Email spam by using the [email address provided by GitHub as a mask to your primary email address](https://github.com/settings/emails) on the GitHub account.  The mask address is of the form `***+github-account@noreply.github.com`.

??? HINT "Avoid use of Company Email address"
    Once you leave a company that email is not verifyable by the sharing service.  Removing the company email address from your account will no longer associate commits with your shared service account.

??? HINT "SSH Key Passphrase"
    Practicalli recommends setting a passphrase when generating an SSH key, to add an extra layer of security.

    The passphrase can be added to the operating system key ring, unlocking the key when logging into the operating system account.

[:fontawesome-brands-github: Generate an SSH key - GitHub](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/){target=_blank .md-button}

[:fontawesome-brands-github: Add SSH key to GitHub account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/){target=_blank .md-button}


## Add SSH key to Keychain

Add Git SSH key passphrase to Operating System keychain to avoid typing in the passphrase each time a Git command interacts with a remote repository.

=== "Linux"

    [Add the SSH private key to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux) to avoid typing in the passphrase each time.  Logging into the operating system with the user account will unlock the key ring and enable access to the passphrase.

    !!! NOTE ""
        ```shell
        ssh-add ~/.ssh/id_ed25519
        ```

    > Ubuntu desktop has a key-ring tool which will display a pop-up dialog to save the passphrase to the key-ring the first time the SSH key is used. Once saved, the key is unlocked when login into the desktop.

=== "MacOSX"

    [Add SSH key passphrase to MacOSX Keychain](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent){target=_blank .md-button}

    !!! NOTE ""
        ```shell
        ssh-add --apple-use-keychain ~/.ssh/id_ed25519
        ```

    Edit the `~/.ssh/config` file and add/modify to include the following configuration

    !!! EXAMPLE "SSH key Key Chain Git Configuration"
        ```shell
        Host github.com
          AddKeysToAgent yes
          UseKeychain yes
          IdentityFile ~/.ssh/id_ed25519
        ```

    If there is an issue with the passphrase, delete the key passphrase using the MacOSX Keychain Access App.  Select `Local Items` to see a list of keys that includes the SSH key.  Select the key to show a menu that allows deletion.

    The command line terminal can be used to delete keys from the MacOSX keychain using the ssh-add command with the -d keyname to delete a specific key or -D option to delete all user added keys.

=== "Termux"
    Termux does not provide a persistent keyring, so an SSH agent can be run manually to manage the SSH key passphrase for the current terminal session.

    Start the agent.

    ```shell
    ssh-agent
    ```

    Capture the details of the agent in the terminal shell

    ```shell
    eval $(ssh-agent)
    ```

    Add the SSH key passphrase to the agent, allowing the SSH key to be used without repeatedly entering the passphrase until the terminal session is closed.

    ```shell
    ssh-add ~/.ssh/engineering
    ```


## Commit signing with SSH Key

Automatic signing each commit with the authors private key ensures traceability of all changes in the Git repository (prevents commit spoofing).  Every company that deals with sensitive data should ensure all commits are signed to provide accountability for all code and configuration commits.

> [SEGAS-00009](https://engineering.homeoffice.gov.uk/standards/signing-code-commits/) is a United Kingdom Home Office engineering standard that requires all commits be signed.

A public SSH key can be registered with a GitHub account as a signing key which is used to validate commits cryptographically signed by the corresponding private key.

??? INFO "SSH Key for Authorization and Signing"
    An SSH key can be registered as both an authorization key used to access a remote repository securely and a signing key to validate commits.

    For extra security, use a separate SSH key for authorization and signing.

Use an existing SSH key to sign commits and tags, or generate a new one specifically for signing.

Configure Git client to use SSH to sign commits and tags for all local repositories.

Add the public keys used for signing commits to an [allowed-signatures](#allowed-ssh-keys) file to see confirmation of the private key used for signing the commit.  Most Git clients will show this information.

??? EXAMPLE "Git Configuration SSH Key signing"
    ```config
    ## ------ Git Behaviour ------ ##
    [commit]
      # Automatically sign every commit
     gpgsign = true

    [tag]
      # Automatically sign every tag
     gpgsign = true

    # SSH Key signing
    [user]
     signingkey = ~/.ssh/id_ed25519.pub
    [gpg]
     format = ssh
    [gpg "ssh"]
     allowedSignersFile = ~/.config/git/allowed-signatures
    ```

Configure SSH key as signing format

!!! NOTE ""
    ```shell
    git config --global gpg.format ssh
    ```

Specify the file that contains the public SSH key to use for signing

!!! NOTE ""
    ```shell
    git config --global user.signingkey $HOME/.ssh/id_ed25519.pub
    ```

Automatically sign commits and tags when creating a commit

!!! NOTE ""
    ```shell
    git config --global commit.gpgsign true && \
    git config --global tag.gpgsign true
    ```

### Allowed SSH keys

The `--show-signature` flag with Git `log` and `show` commands checks the contents of the `gpg.ssh.allowedSignersFile` to know which keys are valid

Create an `$HOME/.config/git/allowed-signatures` file to list the SSH keys that you wish to define as allowed to sign commits.

Each key entry should start with the email address used for commits, followed by the full public key value (which also ends with the email)

Set the `gpg.ssh.allowedSignersFile` file in the Git Configuration

!!! NOTE ""
    ```shell
    git config gpg.ssh.allowedSignersFile "$HOME/.config/git/allowed-signatures"
    ```

??? HINT "SSH keys on multiple machines"
    When using different SSH keys across multiple computers, add all public keys to the `allowed-signatures` file.

    Use a secret GitHub gist if you do not wish to add public keys to a shared git repository for the Git configuration.

## Clone aliases for a GitHub domain

Define a short-cut alias to simplify the URL argument for the repository when using the Git clone command,
e.g `git clone p:clojure-cli-config` rather than `git clone git@github.com:practicalli/clojure-cli-config`

!!! EXAMPLE "Git Clone alias"
    ```shell title=".config/git/config"
    # Clone short-cuts
    [url "git@github.com:practicalli/"]
     # git clone p:repo-name
     insteadOf = p:
    ```

## Diff 3 Support

Diff 3 standard included the parent of two changes in conflict, providing additional context when deciding which change should take precedence

```
git config --global merge.conflictstyle diff3
```

This command adds a `conflictstyle` entry in the `[merge]` section of the Git configuration file.

```
[merge]
    # Include common parent when merge conflicts arise
    conflictstyle = diff3
```

Magit supports the Diff3 standard, so a common parent will be shown when this feature is enabled.

![Git Diff3 standard supported by Magit in Spacemacs](https://github.com/practicalli/graphic-design/blob/live/editors/spacemacs/screenshots/spacemacs-magit-diff3-merge-parent-empty.png?raw=true)
