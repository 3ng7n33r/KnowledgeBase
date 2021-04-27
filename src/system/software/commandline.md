## commands

### RTFM

$ man man - manual page about how to use manual pages

$ man -f /command/ - shows all entries for command, if there are multiple, the first manpage will be opened by default. Open other sections by putting thee number before the command $ man 2 mkdir

$ man -k /keyword/ - searches for a keyword inside the man pages

### Loops
You can write for loops in bash. Example:

    for x in {0..9}; do file ./-file0$x; done
   The syntax is:

    for VARIABLE in N; do command to $Variable; done

### random stuff
xargs will perform an operation to every argument coming from the pipe operator. For example:

    find . | xargs file
  "find ." will return every file in the current directory (.), the pipe operator passes that command to xargs and xargs performs the file operation on every entry

ctrl + r - reverse search (look through history of commands)

history - see former commands

cp (file) (destination) - copy files to destination, uses regex for files (*.jpg, [cf]at.exe …)

mv - move (same syntax as cp)

rm - remove (-f force, -i -r (dangerous!))

rmdir - remove directory

man (command) - show manual of command (whatis works too)

alias name = “command” - give extra name to command

unalias name - removes that alias

~/.bashrc is the default file to save aliases permanently. This file checks for the existence of .bash_aliases and if it exists will load those instead.
If you want to chain or pipe commands in an alias you have to create a function within the alias. with $1 you can also pass parameters. I added this shortcut to quickly upload to github:

    alias gitupload='_gitupload(){ git add . ; git commit . -m "$1" ; git push; }; _gitupload'


cat - [can do a lot of things](https://www.tecmint.com/13-basic-cat-command-examples-in-linux/)

$ ln -s filename linkname - creates shortcut (softlink) to filename

$ ln filename hardlinkname - creates hardlink, a linked copy of the file, it has the same inode number and is therefore identical, changes on one change both, however, one can be deleted uniquely, reducing the files link count. An inode only gets deleted when the link count is zero and the og file gets deleted

  

pstree -p - shows tree of running processes

  

$ strace /command/ - In the simplest case strace runs the specified command until it exits. It intercepts and records the system calls which are called by a process and the signals which are received by a process.

$ lsmod - lists currently loaded kernel modules

  
  

echo Hi > xyz.txt - creates xyz and writes Hi into, if exists it overwrites the data

echo Hi >> xyz.txt - creates xyz and writes Hi into, if exists it appends the data

  

< - redirect stdin (cat < peanuts.txt > banana.txt writes peanuts into banana)

  

2> - writes contents of error stream into destination (0 stdin, 1 stdout, 2 stderr)

ls /fake/directory > peanuts.txt 2>&1 - writes stderr and stdout into peanuts.txt

ls /fake/directory &> peanuts.txt - (same as line before)

  

tail -f /var/log/syslog - tail shows last lines of file, -f is follow flag means in this context that it monitors the end of the file, syslog is the system log file, with this command we can create a terminal that tracks all system acitvity

  

env | grep -i user - grep searches files for patterns, i is the insensitive flag, this command takes the ouptut of the environment variables and searches it for the word user, grep accepts regular expressions

  

$ dd if=/home/pete/backup.img of=/dev/sdb bs=1024M count=2 - copies 2048Mbyte of backup.img to /dev/sdb

  

### list devices

$ lsusb - Listing USB Devices

$ lspci - Listing PCI Devices

$ lsscsi - Listing SCSI Devices

  

### Ownership and permissions

  

ls -l /etc/shadow - shows permissions of shadow file (where passwords are stored) ( r - read, w - write, e - execute, s - [SUID](https://www.linuxnix.com/suid-set-suid-linuxunix/) Set user id allows to inherit user permission from a program

  

$ chmod 755 myfile - changes permissions to myfile to read(4)/write(2)/execute(1)(=7) for active user - read execute (5) for current group and read execute for everyone else, a leading 4 (4755) adds user Suid permission (s), adds group SGID 2755 (s in group field), 1 adds sticky (only owner and root can delete 1755)(t))

sudo chmod u+s myfile - adds Suid permission for user to myfile (SGID is the group equivalent g+s)

  
  

$ sudo chown patty myfile - changes owner of myfile to patty

sudo chgrp whales myfile - changes owning group to whales

$ sudo chown patty:whales myfile - changes owner of myfile to patty and group to whales

  

$ umask 021 - takes away permissions, none from user, write from group and execute from others

  

watch -n 1 "ps aux | grep passwd" - not completely sure what it does yet, monitors UID’s?

  

### Processes

ps aux- creates and shows snapshot of processes (-a all processes, -u shows more details, -x shows processes without tty (controlling terminal) aka deamon processes)

top - monitors running processes

  
  
  
  

## flags

-a all (good with show files (dir or ls))

-r recursive (repeat until not possible to repeat anymore)

-i interactive (give prompts for example when overwriting files)

-l long (gives more details for example in the ps command)

  

### Background

& - at the end of command runs process is Background ( to send a running process to the background, stop it with ctrl + z and enter $ bg)

$ fg JOBID to return the job with JOBID to the foreground, if no JOBID is given, the last job started will return to the foreground

## UID

There’s three types of UID’s, effective, real and saved.

Effective is the UID of the process owner, used for example if an application with SUID rights is executed, effective UID is the UID of the user that launched the process. If no process with SUID permissions is launched, these UID’s are the same, there is also the saved UID which allows a process to swtich between effective and real

  

## Processes

PPID - Parent process id - when a process is called, an existing process is cloned and run as a child (With its own Process ID). The original, now parent process has an PPID. PPID 1 is the init process that handles the os. It is the first process created by the kernel when the system boots up and can only be terminated by a system shutdown

  

_exit is the system call to terminate a process. After it is invoked, the system will wait for the exit status (mostly 0 for success) and then wait until the process is terminated and the space is free again. If the parent process terminates before the child, the child process is now an orphan process that will be handled by init (the mother of all processes)

When a child process terminates without the wait having been called on the parent, this process becomes a zombie. It does not use ressources anymore but it’s still on the process table and as space there is limited, zombies should be avoided (a few are ok). Zombies terminate after the parent process called wait (if there’s no parent, it’s init)

Processes communicate using Signals

kill PID - Sends terminate signal to given process id (find with $ps | grep NAME)

kill -9 PID - the actual kill command. kills process without wait or cleanup

  

Only one process can use the cpu at once. This is called a time slice. The kernel manages when each process can use the cpu. Processes can tell the kernel how much they need the cpu with a value called niceness. A high niceness value means the process let’s less nice processes use the cpu before. The value can be negative.

with the $ nice command the niceness of a new process can be set, $ renice sets the niceness of an existing process

  

Everything in linux is a file and so are processes. Process information is stored in /proc

Find out more about the status of a process with cat /proc/(PID)/status. This is how the kernel sees the system.

  

## Packages

$ gzip file - zips file into archive file.gz

$ gunzip file.gz - reverses zip

  

$ tar cvf archive.tar file1 file2 - tar can zip multiple files the flags are (create, verbose, filename (filename comes after this flag, archive here))

$ tar xvf archive.tar - x is the extract flag

  

A common technique is to bundle files with tar and then zip them into an archive.tar.gz. Tar lets you use the z flag to automatically zip or unzip an archive

$ tar czf myfile.tar.gz

$ tar xzf file.tar

  

$ dpkg X filename - debian package manager, X can be i for install, r for remove or l for list

  

$ apt install - install package

$ apt remove - remove package

$ apt update - update the package repository

$ apt upgrade - upgrade installed packages if possible

$ apt show packagename - show information about packagename

  

### Compile from sourcecode

follow this [guide](https://linuxjourney.com/lesson/compile-source-code) (!Attention - use checkinstall )

  

## partitions

$ sudo parted -l - shows system partitions

$ sudo parted - starts the command line partition program (gparted for GUI)

(parted) select /dev/sda1 - selects device named /dev/sda1

(parted) print - shows partition table of selected device

(parted) mkpart [part-type name fs-type] start end - creates a primary partition (MBR) from start point 123 to end point 456

  

$ fsck /name/ - filesystem check, make sure /name/ is unmounted

  
  

$ df -h - show info about mounted devices

$ df -i - show info about inodes

$ ls -li - shows inode numbers of current directory

$ du -h - shows disk usage of current folder

$ sudo blkid | grep “UUID”- show UUID of mounted devices

  

devices mounted at startup are collected in /etc/fstab (filesystem table) show with $ cat /etc/ fstab, explanation of values [here](https://linuxjourney.com/lesson/etc-fstab-file-system-table)

## Terminology

Process - A process is an instance of a running program, defined by allocated memory, cpu and I/O

  

Master Boot Record MBR - Used to be standard partition table (BIOS), can have a max of four primary partitions, if more are needed 1 single extended partition can be created and inside the extended partition, there can be logical partitions limited by the alphabet for windows and to 63 for linux.

  

GPT - (GUID(Global unique id) Partition Table) new standard (UEFI instead of BIOS)

Screenshot of Laptop file system (sudo parted -l):

![](https://lh5.googleusercontent.com/xaSEHOExmOaMuei9bkGsJiBRlor497-kvgV2xG6qCr4N5Nesrrw2nm7W_HLUOsO2Hu9QSnLU1PXaEQ0OPrO-7naTvfRfQ-GUA60Cgfy_zDJuEKYm77rk0pRboe6nSq88QTT7yIA1)

  

Inodes - Index nodes, the inode table is the database of the filesystem, collecting all information about a file including the location in the memory, more info [here](https://linuxjourney.com/lesson/inodes)

  

Kernel modules - are modules that get added to the kernel. they can be loaded during runtime or added to modprobe to run them during startup. See [here](https://linuxjourney.com/lesson/kernel-modules)

  

udev - a deamon that dynamically creates and removes device files according to the rules in etc/udev/rules.d the devices can be found in the dev folder and can be checked with the $ udevadm info --query=all --name=/dev/sda command
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1ODAyNjI5ODUsLTE4NDY3NzI2NDAsLT
QwODA1NTM5OCwxMjY3MTEzMjYwXX0=
-->