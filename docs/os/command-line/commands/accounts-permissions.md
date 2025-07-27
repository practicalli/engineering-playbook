# Accounts and Permissions

Linux file system access is based on user account and group account membership.

Read, Write and eXecute authorisation is defined with respect to the user (u), group (g) and all accounts (a)

`whoami` shows the currently-logged in user account name.


## Elevated privileges

su switch to another user account, allowing different access permissions for the file system.  `su` is most commonly used to switch to the root user account.

```shell
su -
```

The operating system prompts for the account password that is being accessed.

`sudo` temporarily elevates privileges to that of the root account, allowing access to files and commands that would otherwise be declined.

For example, run the Debian Linux apt tool to update software versions.

```shell
sudo apt upgrade
```


## Change access permissions

`chmod` changes access permissions of files or directories: chmod [options] [permission] [file-or-directory]

Set owner access to read, write and access directories.

```shell
chmod u+rwX ~/projects
```

Set permissions literally:

```shell
chmod -rwx---r-– file.txt
```

The second, third and fourth position sets permission for the owner.  The next three the group and last three any other user.

!!! INFO "Directory and file permissions"
    read (r), write (w), and execute (x)

    owner (u), a group (g), or other (o) accounts



## Change ownership

`chown` changes ownership of files, directories or symbolic links: chown [options] newowner:newgroup file1 file2

Assign a user as the new owner of an item. Leave the group name empty to set the same as the user account.

```shell
chown practicalli ~/projects
```

Omit user account name to make all group members the owner


```shell
chown :hackers ~/projects
```


## Account management

useradd creates a new account or update groups assigned to a user: useradd [options] new_username

By default, the useradd command doesn’t prompt you to give the new user a password. You can add or change it manually later with the passwd command:

passwd new_username

userdel deletes a given account name and requires [elevated privileges](#elevated-privileges)

To set up a password and other details during the account creation process, use the adduser command instead.
