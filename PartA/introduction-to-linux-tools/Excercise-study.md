# Studied contents while doing excercise

## Commands

### `free`

displays amount of free and used physical and swap memory

### `vmstat`

reports information about processes, memory, paging, block 10, traps, disks and cpu activity

#### `-f`

displays the number of forks since boot

### `pidstat`

used for monitoring individual tasks currently being managed by the Linux kernel

- install by command `apt install sysstat`

#### `-w`

report task switching activity

### `ps`

#### Terms

- <defunct>: either completed its task or been corrupted or killed, but its child processes are still running
- TTY: terminal type that user is logged into
- TIME: amount of CPU in minutes and secods that process has been running
- CMD: name of the command that launched the process

### Internal cmds and external cmds

#### Internal commands

commands which are built into the shell

- executable already exist, shell just execute this
- `type (cmd)` results in `(cmd) is a shell builtin`

#### External commands

commands which aren's built into the shell

- shell looks for its path
- implemented by shell

## `/proc` files

to read files in `/proc`: `more /proc/(file)`

### /stat

- ctxt: number of context switches since the system boot
- processes 86031: number of forks since boot

### `/proc/PID/fd`

access set of file descriptors open in a process

- show one entry for each file which the process has open, named by its file desciptor
- to view file descriptors: `ls -trn /proc/PID/fd`
- `0`: stdin
- `1`: stdout
- `2`: stderr

## Characters

### Background process, `&`

- control operator
- if command is terminated by `&`, the shell executes the command in the background in a subshell
	- does not wait for the command to finish
	- return status is 0

### Pipe, `|`

- Linux systems allow the stdout of a command to be connected to the stdin of another command
- `|`: pipe character, used to combine two or more commands
	- ouput of one command act an input to another command
	- unidirectional; flows form left to right