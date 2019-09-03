# ps command

## Show All Running Processes in Linux using ps/htop commands
### Intorduction for process
A process is nothing but tasks within the Linux operating system. A process named httpd used to display web pages.<br> Another process named mysqld provides database service. You need to use the ps command. <br>It provides information about the currently running processes, including their process identification numbers (PIDs). Both Linux and UNIX support the ps command to display information about all running process. <br>The ps command gives a snapshot of the current processes. If you want a repetitive update of this status, use top, atop, and htop command as described below.

## Linux commands show all running processes in Linux

Apart from ps command, you can also use the following commands to display info about processes on Linux operating systems:

- **top** command : Display and update sorted information about processes.
- **atop** command : Advanced System & Process Monitor.
- **htop** command : Interactive process viewer.
- **pgrep** command : Look up or signal processes based on name and other attributes.
- **pstree** command : Display a tree of processes.


## Command examples

* To see every process on the system using standard syntax:
  * ps -e
  * ps -ef
  * ps -eF
  * ps -ely

- To see every process on the system using BSD syntax:
  - ps ax
  - ps axu
- To print a process tree:
    -  ps -ejH
    - ps axjf

- To get info about threads:
    - ps -eLf
    - ps axms

- To get security info:
    - ps -eo euser,ruser,suser,fuser,f,comm,label
    - ps axZ
    - ps -eM

- To see every process running as root (real & effective ID) in user format:
    - ps -U root -u root u

- To see every process with a user-defined format:
    - ps -eo pid,tid,class,rtprio,ni,pri,psr,pcpu,stat,wchan:14,comm
    - ps axo stat,euid,ruid,tty,tpgid,sess,pgrp,ppid,pid,pcpu,comm
    - ps -Ao pid,tt,user,fname,tmout,f,wchan

- Print only the process IDs of syslogd:
    - ps -C syslogd -o pid=

- Print only the name of PID 42:
    - ps -q 42 -o comm=
