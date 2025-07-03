# File System


- [List files and directories](#list-files-and-directories)
- [find files and contents]()



## List files and directories

`ls` command lists the contents of the current directory or the specified path.

`ls -l` shows the meta data of files, e.g. ownership, permissions, etc.

`ls -a` includes hidden dot-files in the list, e.g. `.git`


`tree` shows a recursive list of files and directories, e.g. show the whole contents of a code project.

`pwd` shows the full path of the current working directory


## Navigate the file system


`cd` to change to a different directory by giving the relative or full path

```shell
cd [path_or_directory]
```

Short-cuts:

- `cd` move to the current user’s home directory
- `cd ..`  – move up a directory and can be used with a relative path `cd ../sibling-directory`
- `cd –` – move to the previous directory

## Change files and directories

`touch filename` creates a new file using the given name (assuming the file doesn't already exist)

`mkdir path/to/target_folder/new-folder-name` creates a new directory using a relative or full path, the current user has read, write, and execute files permissions in the new directory (`-m` flag or `chmod` to alter permissions)

`cp` copies files using the form cp file1 file2 [target_path], Use the `-R` flag to recursively copy a directory and its contents (including sub-directories)

`mv`  moves (optionally rename) a file or directory to another location using the form mv file_or_directory [target_directory]

`rm` command deletes files from a directory. You must have the write permission for the folder or use sudo. Uses the form: rm [options] file1 file2

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

tar -xf archive.tar extracts everything from the archive, `-C` extracts to a given path.

tar can change the contents of an archive using `--concatenate`, `--delete`, `--append` and `--update` flags.


zip is a universal archive tool that compresses one or multiple files into a `.zip` archive, reducing the size of text based files. Binary files may result in a slightly bigger archive.

`zip [options] zip_file_name <directory and/or files>` creates a compressed archive.

`unzip [options] zip_file_name` extracts all the files form the archive.


## Find files and contents


The find command is one of the most useful Linux commands, especially when you're faced with the hundreds and thousands of files and folders on a modern computer. As its name implies, find helps you find things, and not just by filename.

Whether you're on your own computer or trying to support someone on an unfamiliar system, here are 10 ways find can help you locate important data.

[ Keep your most commonly used commands handy with the Linux commands cheat sheet. ]
1. Find a single file by name

When you know the name of a file but can't remember where you saved it, use find to search your home directory. Use 2>/dev/null to silence permission errors (or use sudo to gain all permissions).

$ find / -name "Foo.txt" 2>/dev/null
/home/seth/Documents/Foo.txt

2. Find a single file by approximate name

If you can't remember the exact name of the file, or you're not sure whether you capitalized any characters, you can do a partial and case-insensitive search like this:

$ find / -iname "*foo*txt" 2>/dev/null
/home/seth/Documents/Foo.txt
/home/seth/Documents/foo.txt
/home/seth/Documents/foobar.txt

3. Find everything

The ls -R command lists the contents of a directory recursively, meaning that it doesn't just list the target you provide for it, but also descends into every subdirectory within that target (and every subdirectory in each subdirectory, and so on.) The find command has that function too, by way of the -ls option:

$ find ~/Documents -ls
3554235 0 drwxr-xr-x [...] 05:36 /home/seth/Documents/
3554224 0 -rw-rw-r-- [...] 05:36 /home/seth/Documents/Foo
3766411 0 -rw-rw-r-- [...] 05:36 /home/seth/Documents/Foo/foo.txt
3766416 0 -rw-rw-r-- [...] 05:36 /home/seth/Documents/Foo/foobar.txt

Notice that I don't use 2>/dev/null in this instance because I'm only listing the contents of a file path within my home directory, so I don't anticipate permission errors.
4. Find by content

A find command doesn't have to perform just one task. In fact, one of the options in find enables you to execute a different command on whatever results find returns. This can be especially useful when you need to search for a file by content rather than by name, or you need to search by both.

$ find ~/Documents/ -name "*txt" -exec grep -Hi penguin {} \;
/home/seth/Documents/Foo.txt:I like penguins.
/home/seth/Documents/foo.txt:Penguins are fun.

[ Learn how to manage your Linux environment for success. ]
5. Find files by type

You can display files, directories, symlinks, named pipes, sockets, and more using the -type option.

$ find ~ -type f
/home/seth/.bash_logout
/home/seth/.bash_profile
/home/seth/.bashrc
/home/seth/.emacs
/home/seth/.local/share/keyrings/login.keyring
/home/seth/.local/share/keyrings/user.keystore
/home/seth/.local/share/gnome-shell/gnome-overrides-migrated
[...]

As long as you're using the GNU version of find, you can include multiple file types in your search results:

$ find ~ -type f,l -name "notebook*"
/home/seth/notebook.org
/home/seth/Documents/notebook-alias.org

6. List just directories

A shortcoming of the ls command is that you can't filter its results by file type, so it can be noisy if you only want a listing of directories in a path. The find command combined with the -type d option is a better choice:

$ find ~/Public -type d
find ~/Public/ -type d
/home/seth/Public/
/home/seth/Public/example.com
/home/seth/Public/example.com/www
/home/seth/Public/example.com/www/img
/home/seth/Public/example.com/www/font
/home/seth/Public/example.com/www/style

7. Limit listing results

With hundreds of files in a default user directory and thousands more outside of that, sometimes you get more results from find than you want. You can limit the depth of searches with the -maxdepth option, followed by the number of directories you want find to descend into after the starting point:

$ find ~/Public/ -maxdepth 1 -type d
/home/seth/Public/
/home/seth/Public/example.com

8. Find empty files

Sometimes it's helpful to discover empty files as a way to declutter:

$ find ~ -type f -empty
random.idea.txt

Technically, you can use find to remove empty files, but programmatic removal of files is dangerous. For instance, if you forget to include -type f in a search for empty files, you get directories in your results. By adding a delete flag, you would remove potentially important directory structures.

It's vital to compose your find command and then verify the results before deleting. Furthermore, a misplaced delete flag in find can delete results before qualifying them (in other words, you can delete directories in a command intended to delete only files by placing the delete flag before the type flag).

I prefer to use xargs or Parallel and a trash command on the rare occasion that I remove files with find.
9. Find files by age

The -mtime option allows you to limit a search to files older than, but also files newer than, some value times 24.

$ find /var/log -iname "*~" -o -iname "*log*" -mtime +30

The + before the -mtime number doesn't mean to add that number to the time. It's a conditional statement that matches (in this example) a value greater than 24 times 30. In other words, the sample code finds log files that haven't been modified in a month or more.

To find log files modified within the past week, you can use the - conditional:

$ find /var/log -iname "*~" -o -iname "*log*" -mtime -7
/var/log/tallylog
/var/log/cups/error_log
/var/log/cups/access_log
/var/log/cups/page_log
/var/log/anaconda/anaconda.log
/var/log/anaconda/syslog
/var/log/anaconda/X.log
[...]

You already know about the -ls flag, so you can combine that with these commands for clarity:

$ find /var/log -iname "*~" -o -iname "*log*" -mtime -7 -ls
-rw-------  1 root root            0 Jun  9 18:20 /var/log/tallylog
-rw-------  1 root lp      332 Aug 11 15:05 /var/log/cups/error_log
-rw-------  1 root lp      332 Aug 11 15:05 /var/log/cups/access_log
-rw-------  1 root lp      332 Aug 11 15:05 /var/log/cups/page_log
-rw-------  1 root root  53733 Jun  9 18:24 /var/log/anaconda/anaconda.log
-rw-------  1 root root 835513 Jun  9 18:24 /var/log/anaconda/syslog
-rw-------  1 root root  21131 Jun  9 18:24 /var/log/anaconda/X.log
[...]

10. Search a path

Sometimes you know the directory structure leading up to a file you need; you just don't know where the directory structure is located within the system. To search within a path string, you can use the -ipath option, which treats dots and slashes not as regex characters but as dots and slashes.

$ find / -type d -name 'img' -ipath "*public_html/example.com*" 2>/dev/null
/home/tux/Public/public_html/example.com/font

Found it

The find command is an essential tool for a sysadmin. It's useful when investigating or getting to know a new system, finding misplaced data, and troubleshooting everyday problems. But it's also just a convenience tool.
