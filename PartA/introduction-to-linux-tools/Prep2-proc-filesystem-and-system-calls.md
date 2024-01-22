# Preparation 2: `/proc` filesystem and system calls of linux

[Reference](https://man7.org/linux/man-pages/man5/proc.5.html)

## `/proc` filesystem

virtual file system that provides an interface for kernel

- created on the fly(즉시) when the system boots and is dissolved at the time of system shutdown
- contains information about the processes
- regarded as a control and infomation(about system and processes) center for the kernel
- provides communication medium between kernel space and user space
- mounted automatically by the system

### `/proc/meminfo`

reports statistics about memory usage on the system

- used by `free` to report the amount of free and used memory
	- both physical and swap
	- including shared memory, buffers used by the kernel
- each line: parameter name, value of parameter, option unit

#### `free`

command that display amount of free and used memory in the system

- systax: `free (option)`

### `/proc/cpuinfo`

collection of CPU and system architecture dependent items

- a different list for each supported architecture
- entry: processor and bogomips
	- processor: gives CPU number
	- bogomips: system constant that is calculated during kernel initialization
- SMP machines have information for each CPU

#### `lscpu`

command that display information about CPU architecture

- gather information from `/cpuinfo`

### `/proc/pid/status`

provide information in `/proc/pid/stat` and `/proc/pid/statm` in human friendly format

- `/proc/pid/statm`: provide information about memory usage

### `/proc/pid/stat`

status information about process

- used by `ps`

### `/proc/pid/limits`

displays soft limit, hard limit, units of measurements for each process's resource limits

- protected to allow reading by the real UID of the process
- since linux 2.6.36 readable by all users on the system

### `/proc/pid/maps`

contains currently mapped memory regions and their access permissions

- permission to access: governed by a ptrace access mode PTRACE_MODE_READ_FSCREDS check

#### `ptrace`

system call that provides a means by which one process may observe and control the execution of another process

- examine and change the tracee's memory and registers
- primarily used to implement breakpoint debugging and system call tracing

## System calls of Linux

### Process control

#### fork()

create new process

#### exit()

terminate process execution

#### exec()

execute process

### File management

#### open()

just open file

#### read()

open files in reading mode

- cannot edit
- mutiple processes can execute this on the same file simultaneously

#### write()

open files in writing mode

- can edit
- mutiple processes can execute this on the same file simultaneously

#### close()

close open files

### Device management

does job of device manipulation

- reading from device buffers
- writing into device buffers

#### ioctl()

Input Output ConTrL

- for device specific io operations and other operations which cannot be expressed by regular system calls

### Information maintenace

handle information and its transfer between OS and user program

- OS keeps information about all processes
- system calls access the informations that OS keeps

#### getpid()

get PID

- return PID of calling process
- no return value for an error; shall always be successful

#### alarm()

sets an alarm clock for the delivery of a signal that when it has to be reached

- arranges for a signal to be delivered to the calling process

#### sleep()

suspends the execution of the currently running process for some interval of time

- during interval, another process is given chance to execute

### Communication

models
1. message passing
2. shared memory

#### pipe()

communicate between different linux processes

- mainly used for inter-process communication
- used to open file descriptors

#### shmget()

SHared Memory seGmEnT

- used for shared memory communication
- used to access the shared memroy and the messages in order to communicate

#### mmap()

- used to map or unmpa files or devices into memory
- responsible for mapping the content of the file to the virtual memory space of the process