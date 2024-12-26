# Git Personal Access Token

A personal access token is required when accessing a remote repository over HTTPS.

## Generate a token

Visit the remote repository service and generate a personal access token with at least `repo` permission.

* [GitHub personal access token documentation](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
* [GitLab personal access token documentation](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html#create-a-personal-access-token)

Whilst the token could be added to the `~/.gitconfig`, as this file is plain text it is not particularly secure (especially if committed into a dotfiles repository and shared).

```shell
git config --global oauth.token "tokens-in-plain-text-files-are-not-very-secure"
```

To provide greater security when using the token, consider using the [Git Credential Manager](https://github.com/GitCredentialManager/git-credential-manager).  

??? HINT "Magit Forge uses personal access token"
    [Magit Forge also requires a personal access token](forge-configuration.md), although this can be saved in the encrypted file `~/.authinfo.gpg` for greater security.  The Magit Forge token includes permissions required to access remote repositories over HTTPS

??? HINT "Octo plugin for Neovim uses token from GitHub CLI"
