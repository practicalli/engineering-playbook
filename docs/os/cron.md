# Cron scheduled tasks

`cron` is daemon that automatically runs a script or command at predefined time. cron is started automatically from /etc/init.d on entering multi-user runlevels.

`crontab` is used to create and update the schedule of tasks.

??? INFO "Linux Daemon"
    Daemon is a Linux background system process

??? INFO "Scheduled GitHub workflow"
    Use a `schedule:` directive to add `cron:` events that automatically run on the scheduled time

    [GitHub Docs - Schedule](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule){target=_blank .md-button} 


## Crontab

The crontab is used to automate all types of tasks on Linux systems.



- Cron Concepts
- Setup Crontab Access for a User Account
- Handle errors with cronjobs
- Creating cronjobs

Difference between Cron, Crontab and Cron Job

Element	Linux Name	Meaning
- crontab: 	rows in a table for each cron job, each ‘*’ asterisk represents a segment of time and a corresponding column in each row.
- Cron Job:	specific task to be performed described in a row, paired with its designated time id


```shell
ps aux | grep crond
```

This command will search current processes for all users and return any instances of ‘crond’.

```shell
ps ux | grep crond
practicalli  8942  0.0  0.0  18612   840 pts/0    S+   02:16   0:00 grep --color=auto crond
```

The daemon is running for the practicalli user account.

## Crontab syntax


crontab [options]

* * * * *  updatedb
OR 
* * * * * <path/to/script>


* * * * *
| | | | |
| | | | |-> Day of the week (0-6)
| | | |---> Month of the year (1-12)
| | |-----> Day of the month (1-31)
| |-------> Hour (0-23)
|---------> Minute (0-59)


List the crontab entries for the current user account.

```shell
crontab -l
```

Edit entries in the crontab

```shell
crontab -e
```

Select a tool to edit the crontab for the first time

```shell
no crontab for practicalli - using an empty one

Select an editor.  To change later, run 'select-editor'.
  1. /bin/nano        <---- easiest
  2. /usr/bin/vim.basic
  3. /usr/bin/kak
  4. /usr/bin/vim.tiny
  5. /usr/bin/code
  6. /bin/ed

Choose 1-6 [1]: 
```


## Examples

Redirect cron output to a file

```shell
* * * * * sh /path/to/script.sh &> log_file.log
```

log errors

```shell
* * * * * sh /path/to/script.sh >> /tmp/cron.txt 2>&1
```

drop out debug logs

```shell
* * * * * /bin/bash -x /path/to/script.sh >> /tmp/cron.txt 2>&1
```
## Tips

- echo the date and time at start of script

