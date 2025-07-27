# System

View system resources (CPU, memory, disk, etc).

View and manage processes and kill those using excessive resources.


## Resources

`top` displays all running processes in the system and their resource consumption (cpu, memory)

`top -p PID` for a specific process.

`htop` displays and manages processes in the operating server, `–-tree` shows process hierarchical view.


## Processes

`ps` summarizes the status of all running processes in your Linux system at a specific time

`ps ru` shows only running processes for the current user

`time` measures the execution time of commands or scripts to gain insights into system performance.

```shell
time command-or-script && another-command-or-sript
```


`jobs -l` show tasks or commands running in the current shell (useful if you forget about background jobs)


Use the kill command to terminate a process using its ID. Here’s the basic syntax:

`kill -9 Process_ID` to kill a specific process (using the ID shown via the `ps` command)

`killall process-name` e.g. `killall chrome`

To obtain the process ID, run the following command:

ps ux

The kill command uses [SIGTERM](https://www.gnu.org/software/libc/manual/html_node/Termination-Signals.html) to allow a process a little time to save data and release resources gracefully before exiting.


## Kernel

`uname` (unix name) displays detailed information about your Linux machine, including hardware, name, and operating system kernel.

```shell-output
❯ uname -a
Linux gkar 6.12.32-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.12.32-1 (2025-06-07) x86_64 GNU/Linux
```

Flags:

-`a` `--all` print all information, in the following order, except omit -p and -i if unknown:
-`s` `--kernel-name` print the kernel name
-`n` `--nodename` print the network node hostname
-`r` `--kernel-release` print the kernel release
-`v` `--kernel-version` print the kernel version
-`m` `--machine` print the machine hardware name
-`p` `--processor` print the processor type (non-portable)
-`i` `--hardware-platform` print the hardware platform (non-portable)
-`o` `--operating-system` print the operating system


## Network

- `hostname` - show or set the system's host name
- `domainname` - show or set the system's NIS/YP domain name
- `ypdomainname` - show or set the system's NIS/YP domain name
- `nisdomainname` - show or set the system's NIS/YP domain name
- `dnsdomainname` - show the system's DNS domain name


`ip address` shows the Network IP addresses available on the operating system


## Services

`systemctl` to list and manage services in the system

```shell
systemctl subcommand [service_name][options]
```

The subcommands represent your task, like listing, restarting, terminating, or enabling the services. For example, we will list Linux services using this:

```shell
sudo systemctl list-unit-files --type service --all
```

`systemctl show` lists all services


??? EXAMPLE "System Control - all information"
    ```shell-output
    ❯ systemctl show
    Version=257.6-1
    Features=+PAM +AUDIT +SELINUX +APPARMOR +IMA +IPE +SMACK +SECCOMP +GCRYPT -GNUTLS +OPENSSL +ACL +BLKID +CURL +ELFUTILS +FIDO2 +IDN2 -IDN +IPTC +KMOD +LIBCRYPTSETUP +LIBCR>
    Architecture=x86-64
    Tainted=unmerged-bin
    FirmwareTimestampMonotonic=17770382
    LoaderTimestampMonotonic=7539019
    KernelTimestamp=Thu 2025-07-03 08:57:35 BST
    KernelTimestampMonotonic=0
    InitRDTimestampMonotonic=0
    UserspaceTimestamp=Thu 2025-07-03 08:57:41 BST
    UserspaceTimestampMonotonic=5422672
    FinishTimestamp=Thu 2025-07-03 08:57:54 BST
    FinishTimestampMonotonic=18590863
    ShutdownStartTimestampMonotonic=0
    SecurityStartTimestamp=Thu 2025-07-03 08:57:41 BST
    SecurityStartTimestampMonotonic=5429873
    SecurityFinishTimestamp=Thu 2025-07-03 08:57:41 BST
    SecurityFinishTimestampMonotonic=5431546
    GeneratorsStartTimestamp=Thu 2025-07-03 08:57:41 BST
    GeneratorsStartTimestampMonotonic=5704612
    GeneratorsFinishTimestamp=Thu 2025-07-03 08:57:41 BST
    GeneratorsFinishTimestampMonotonic=5747292
    UnitsLoadStartTimestamp=Thu 2025-07-03 08:57:41 BST
    UnitsLoadStartTimestampMonotonic=5747313
    UnitsLoadFinishTimestamp=Thu 2025-07-03 08:57:41 BST
    UnitsLoadFinishTimestampMonotonic=5856839
    UnitsLoadTimestamp=Thu 2025-07-03 09:23:28 BST
    UnitsLoadTimestampMonotonic=1412993331
    InitRDSecurityStartTimestampMonotonic=0
    InitRDSecurityFinishTimestampMonotonic=0
    InitRDGeneratorsStartTimestampMonotonic=0
    InitRDGeneratorsFinishTimestampMonotonic=0
    InitRDUnitsLoadStartTimestampMonotonic=0
    InitRDUnitsLoadFinishTimestampMonotonic=0
    LogLevel=info
    LogTarget=journal-or-kmsg
    NNames=427
    NFailedUnits=1
    NJobs=0
    NInstalledJobs=349
    NFailedJobs=1
    Progress=1
    Environment=LANG=en_GB.UTF-8 LANGUAGE=en_GB:en PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
    ConfirmSpawn=no
    ShowStatus=no
    UnitPath=/etc/systemd/system.control /run/systemd/system.control /run/systemd/transient /run/systemd/generator.early /etc/systemd/system /etc/systemd/system.attached /run>
    DefaultStandardOutput=journal
    DefaultStandardError=inherit
    WatchdogLastPingTimestampMonotonic=18446744073709551615
    RuntimeWatchdogUSec=0
    RuntimeWatchdogPreUSec=0
    RebootWatchdogUSec=10min
    KExecWatchdogUSec=0
    ServiceWatchdogs=yes
    SystemState=degraded
    DefaultTimerAccuracyUSec=1min
    DefaultTimeoutStartUSec=1min 30s
    DefaultTimeoutStopUSec=1min 30s
    DefaultTimeoutAbortUSec=1min 30s
    DefaultDeviceTimeoutUSec=1min 30s
    DefaultRestartUSec=100ms
    DefaultStartLimitIntervalUSec=10s
    lines 1-62
    ```


## View logs

`watch` run a utility every 2 seconds (or given interval) to monitor changes in the output.

`-n` to specify the delay in seconds.

`-d` highlights changes in the output.


## Shutdown and reboot

`shutdown` to turn off or restart your Linux system at a specific time, `shutdown [option] [time] [message]`.  The message is shown to all users logged into the system.

`-r` to reboot the system.

```shell
shutdown -r now
```
