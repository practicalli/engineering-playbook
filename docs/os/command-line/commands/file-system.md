# File System

View and navigate the contents of the files and directories on the file system.

Create and extract archives of files and directories.

Use `$HOME` and `~` within directory paths as convenient Short-cuts.

!!! TIP "Use `sudo` with these commands to access files & directories with elevated permissions"


## List files and directories

`ls` command lists the contents of the current directory or the specified path.

`ls -l` shows the meta data of files, e.g. ownership, permissions, etc.

`ls -a` includes hidden dot-files in the list, e.g. `.git`


`tree` shows a recursive list of files and directories, e.g. show the whole contents of a code project.

`pwd` shows the full path of the current working directory


## Disk usage

`df -h` shows all disk usage in the operating system, in human readable format

`du -sh` shows summary file use of the current directory (or specified path), including usage for sub-folders


## Navigate the file system

`cd` to change to a different directory by giving the relative or full path

```shell
cd projects/practicalli
```

Short-cuts:

- `cd` move to the current user’s home directory
- `cd ..` move up a directory and can be used with a relative path `cd ../sibling-directory`
- `cd –` move to the previous directory

## Change files and directories

`touch filename` creates a new file using the given name (assuming the file doesn't already exist)

`mkdir path/to/target_folder/new-folder-name` creates a new directory using a relative or full path, the current user has read, write, and execute files permissions in the new directory (`-m` flag or `chmod` to alter permissions)

`mkdir -p path/that/doesnt/exist/yet` the `-p` option will create all the directories that are not already present in the given path.

`cp` copies files using the form `cp file1 file2 [target_path]`. Use the `-R` flag to recursively copy a directory and its contents (including sub-directories)

`mv`  moves (optionally rename) a file or directory to another location using the form `mv file_or_directory [target_directory]`

`rm` command deletes files from a directory. Uses the form: `rm [options] file1 file2`

!!! INFO "Common flags for file and directory commands"
    `-r` (recursive) acts on a directory and its contents, including subdirectories.

    `-i` (interactive) flag prompts for confirmation before running the command

    `-f` (force) to run the command without confirmation


!!! INFO "Shell may be configured to ask for delete confirmation"
<!--- TODO: add link to configure zsh and bash shells to always prompt for delete confirmation --->



## Archives

Tar (tape archive) is the long standing command for creating archives on Unix, optionally compressing the resulting file with one of several algorithms.

`tar -cf tar_file_name.tar <directory and/or files>` creates a new archive, `-c` for create, `-f` to specify archive file name.

`tar -cfz archive.tgz <directory and/or files>` creates a new archive using gzip compression.

Archive options:

- `z` gizip compression, `.tgz` file extension
- `j` bzip2 compression, `.tgj` file extension
- `J` xz compression, `.tgx` file extension

`tar -xf archive.tar` extracts everything from the archive, `-C` extracts to a given path.

`tar` can change the contents of an archive using `--concatenate`, `--delete`, `--append` and `--update` flags.


`zip` is a universal archive tool that compresses one or multiple files into a `.zip` archive, reducing the size of text based files. Binary files may result in a slightly bigger archive.

`zip [options] zip_file_name <directory and/or files>` creates a compressed archive.

`unzip [options] zip_file_name` extracts all the files form the archive.


## Find files and contents


`find` to look for files and directories by a pattern

!!! TIP "Append `2>/dev/null` onto commands to silence permission errors"

Find the exact file name anywhere on the file system (from the `/` root)

```shell
find / -name "secret-sauce.md" 2>/dev/null
```

`-iname` is case insensitive.  Wild-cards `*` can also be used when the exact name is not known.

```shell
$ find / -iname "*secret*md" 2>/dev/null
```

`find` can execute a different command on results returned, e.g searching for files by content rather than name

```shell
$ find ~/Documents/ -name "*md" -exec grep -Hi squirrels {} \;
/home/practicalli/Documents/nature-in-action.md:I love watching squirrels play.
```

`-maxdepth` limits the number of directories to traverse

With hundreds of files in a default user directory and thousands more outside of that, sometimes you get more results from find than you want. You can limit the depth of searches with the -maxdepth option, followed by the number of directories you want find to descend into after the starting point:

```shell
$ find ~/secret-sauce.md -maxdepth 1
```


`-mtime` limits a search to files older or newer than a value times, by increments of 24 hours.

Find files in the current user account home directory, modified in the last 24 hours

```shell
find $HOME -mtime 0
```

Use a `+` before the value of `-mtime` as a conditional, matching files modified before 24 times the value.

Find log file which have not been update for more than a week.

```shell
find /var/log -iname "*~" -o -iname "*log*" -mtime +7
```


The `-` conditional find modified files within 24 hours times the value.

```shell
find /var/log -iname "*~" -o -iname "*log*" -mtime -7
```

`-ls` flag provides a long list showing meta data about the files found

```shell-output
$ find /var/log -iname "*~" -o -iname "*log*" -mtime -7 -ls
-rw-------  1 root root            0 Jun  9 18:20 /var/log/tallylog
-rw-------  1 root lp      332 Aug 11 15:05 /var/log/cups/error_log
-rw-------  1 root lp      332 Aug 11 15:05 /var/log/cups/access_log
-rw-------  1 root lp      332 Aug 11 15:05 /var/log/cups/page_log
-rw-------  1 root root  53733 Jun  9 18:24 /var/log/anaconda/anaconda.log
-rw-------  1 root root 835513 Jun  9 18:24 /var/log/anaconda/syslog
-rw-------  1 root root  21131 Jun  9 18:24 /var/log/anaconda/X.log
```

`-ipath` to search for a path within the file space

```shell
find / -type d -name 'img' -ipath "*public_html/practicalli*"
```
