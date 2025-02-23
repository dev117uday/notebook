# Troubleshooting and Debugging

- **Troubleshooting** : the process of identifying, analyzing and solving problems.
- **Debugging** : the process of identifying, analyzing and removing bugs in a system.

* Troubleshooting is for infrastructure
  * tcpdump, wireshark
  * strace, ltrace
  * ps, top etc
* Debugging : is for Software application
* Debugger : follows the code line by line , inspect changes in variable assignments, interrupt the program when a specific condition is met and more

**Steps to solve any issue**

* Getting Information to understand the problem
* Isolate and finding the root cause
* Performing the necessary remediation
* Document what we do
  * The different things we tested to try
  * Figure out the root cause.
  * The steps we took to fix the issue.

## `strace` & `ltrace`

_// TODO complete this section x_

**strace : to trace system calls made by the program**

* o file.strace : to save output of trace to a file

## Main steps to solving any issue ?

**Questions to Ask before debugging**

![Question to ask before debugging](../../notes-md/gitbook/assets/question_to_ask.png)

- _Isolating the root cause is super important_

# Reproduction Case

##  Refer logging 

* Linux
// TODO get types of logs to investigate in linux
  * /var/log/syslog
  * .xsession-errors
* MacOS : Library/Logs/
* Windows : EventViewer

Our solution dont come up by wandering about things, we have to look at information to plug things into our knowledge graph, looking at error messages or documentation.

- **Get a reproduction, try to isolate the problem**
- **Understanding the root cause is super important**

## Usefull Commands

### `top` & `uptime` Load Average

- Load average : amount of time the process is busy in a minute
- Load average 1 means it was busy for whole min.
- shouldn’t be above the amount of process in computer
- The 3 values indicate `1 min` , `5 min`, `15 min` 
- must be less than CPU cores.

```bash
# uptime
13:36:43 up 6 min,  1 user,  load average: 0.26, 0.12, 0.04 
```

```bash 
#top
top - 15:24:23 up  1:53,  1 user,  load average: 0.08, 0.06, 0.06
Tasks:  44 total,   1 running,  43 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.2 us,  2.2 sy,  0.0 ni, 93.3 id,  1.1 wa,  0.0 hi,  1.1 si,  0.0 st
MiB Mem :   1971.8 total,    845.8 free,    961.2 used,    351.5 buff/cache
MiB Swap:   1024.0 total,   1019.7 free,      4.3 used.   1010.6 avail Mem
```

### `iostat` :
system monitoring tool that reports CPU and I/O statistics for devices and partitions.

```bash
Linux 5.15.167.4-microsoft-standard-WSL2 (skynet-e14-r3)        02/16/25        _x86_64_        (8 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.24    0.00    0.24    0.06    0.00   99.45

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
/dev/sda               0.17        11.45         0.00         0.00      74445          0          0
/dev/sdb               0.03         0.35         0.64         0.00       2292       4184          0
/dev/sdc              12.81       294.03       297.25       197.97    1911161    1932064    1286760
```

### `iotop` :

Utility that displays real-time I/O usage by processes or threads on a Linux system.

```bash 
Total DISK READ:         0.00 B/s | Total DISK WRITE:         0.00 B/s
Current DISK READ:       0.00 B/s | Current DISK WRITE:       0.00 B/s
    TID  PRIO  USER     DISK READ DISK WRITE>    COMMAND                                                                                                                                         1 be/4 root        0.00 B/s    0.00 B/s init
      2 be/4 root        0.00 B/s    0.00 B/s init
      9 be/4 root        0.00 B/s    0.00 B/s init [Interop]
      7 be/4 root        0.00 B/s    0.00 B/s plan9 --control-socket 7 --log-level 4 --server-fd 8 --pipe-fd 10 --log-truncate
      8 be/4 root        0.00 B/s    0.00 B/s plan9 --control-socket 7 --log-level 4 --server-fd 8 --pipe-fd 10 --log-truncate
```

### `vmstat` :

Vmstat is a performance monitoring tool that provides information about processes, memory, paging, block I/O, traps, and CPU activity.
- imp parameter : `wa` -> means wait time
```bash 
procs -----------memory---------- ---swap-- -----io---- -system-- -------cpu-------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st gu
 0  0   4388 236800   3740 532620    0    0   187   174  228    0  0  0 99  0  0  0
```

### `iftop` :
iftop is a network monitoring tool that displays bandwidth usage on an interface by host.

- `time` : to calculate time taken by program to complete
```bash 
uday@skynet-e14-r3:~/notebook$ time

real    0m0.000s
user    0m0.000s
sys     0m0.000s
```

- `kill` command
// TODO get types of kill commands

Like isolating causes, understanding error messages, adding logging information, and generating new ideas for possible failures.