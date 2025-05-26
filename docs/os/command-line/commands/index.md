# Common CLI Commands

    Console and output management:
        cat - Display file contents on the terminal.
        clear - Clear the terminal display.
        echo - Print text or variables in the terminal.
        top - Get information about running processes.
    Creating and exporting environment variables:
        env - Display all environment variables running on the system.
        export - Export environment variables.
        printenv - Print a particular environment variable to the console.
        source - Execute commands stored in a file from within the current shell, or refresh environment variables.
    Working with files and directories:
        cd - Change to another directory.
        cp - Copy the contents of the source directory or file to a target directory or file.
        find - Locate a file or directory by name.
        grep - Search for a string within an output.
        ls - List the contents of a directory.
        mkdir - Create directories.
        more - View and traverse the content of a file or stdout.
        mv - Move or rename files.
        pwd - Get the name of the present working directory.
        rm - Delete files or directories.
        tar - Extract and compress files.
    Accessing command-line help documentation:
        man - Access manual pages for all Linux commands.
    Working with networks on and from a Linux computer:
        curl - Get or post a file to or from the Internet according to a URL.
        ip - Gets the IP information for the physical or virtual machine.
        netstat - Get information about network connections and more.
        ssh - Establish a secure encrypted connection between two hosts over an insecure network.
        wget - Direct download files from the Internet.
    Process management:
        && - Execute commands in a sequence.
        kill - Removes a running process from memory.
        ps - Display active processes.
    System control:
        poweroff - Shut down a computer.
        restart - Restart a computer.
    User management:
        whoami - Display the user ID.

Red Hat Developer cheat sheets give you essential information right at your fingertips so you can work faster and smarter. Easily learn new technologies and coding concepts and quickly find the answers you need.
Excerpt
find

sudo find <starting/directory> -name <file/directory name>

Copy snippet

Finds a file or directory by name.

Example:

The following command finds a file named hostname starting from the root (/) directory of the computer’s filesystem. Note that the command starts with sudo in order to access files restricted to the root user:

$ sudo find / -name hostname

/proc/sys/kernel/hostname

/etc/hostname

/var/lib/selinux/targeted/active/modules/100/hostname

/usr/bin/hostname

/usr/lib64/gettext/hostname

/usr/share/licenses/hostname

/usr/share/doc/hostname

/usr/share/bash-completion/completions/hostname

/usr/share/selinux/targeted/default/active/modules/100/hostname

/usr/libexec/hostname

Copy snippet
grep

grep <search_expression> <input>

$ less -N ~/.bashrc
