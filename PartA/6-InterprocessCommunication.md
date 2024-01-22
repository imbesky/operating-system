# Inter-process communication

- processes often need to communicate in order to accomplish useful tasks

## Shared memory

simplest way to communicate with each other for two processes

### Concept

- default: to separate processes have two separate memory images, do not share memory
	- child process도 fork 시점에만 동일
- processes that wish to share memory request the kernel: `shmget()` system call
	- by providing same key, can get same memory segment in the same region
	- can be used as regular memory with property	; read/write to memory to communicate
	- changes made by one process will be visible to the other and vice versa

### Problem; Synchronization and race condition

if several processes share a memory segment, it is possible for multiple processes to make a concurrent update, overwriting

- can result in a wrong value
- mechanisms like locking should be used

## Signal

used to notify processes of events

### Concept

- signal handler: default code to execute for each signal that every process has
- signal handlers can be overridden
	- process can have own in-built signal handling function
		- can be passed to kernel with signal system call
	- OS invoke new function(overridden signal handler) when process receives a signal
- signals are used(can be sent) to communicate between OS and process, process and other process
	- e.g. kill system call

#### Example

1. user hits Ctrl+C
2. OS sends signal SIGINT to running process to handle keyboard interrupt
3. process receives signal and normal execution of process is halted
4. separate piece of code, signal handler is executed by process

## Socket

mechanism to communicate between two processes

### Concept

- same machine이거나 different machine이거나 상관없음
	- across machines: TCP/UDP socket, Network socket
	- local machine: Unix socket
- widely used for communicating across hosts than across processes on the same host

#### Sequence

1. processes open sockets and connect them to each other
2. messages written into one socket can be read from another
3. OS transfers data across socket buffers

## Pipe

half-duplex connection between two file desciptors

- only can send data one way

### Concept

- data written into one file descriptor can be read out through the other
- pipe system call: a method used by a pair of file descriptors to bound reading/writing
- read end, write end: two ends of pipe
- reading from and writing to a pipe can be blocking or not
	- depends on pipe configuration
- data written to a pipe is stored temporary memory by the OS
	- made available to process that reads from it

#### Regular pipes

when both file descriptors are in same process

- parent and child share file descriptor after fork
- paren uses on end child uses other end

#### Unnamed pipes

anonymous pipe; no way to refer to them outside the proces

- can be used within the same process or within the childern of the process
- parent process can create a pipe and hand over endpoints to children

#### Named pipes

- named pipes or FIFOs enable process to create a pipe with specified name
	- endpoints of the pipe can be accessed outside the process
- two different processes can get to end points of the same pipe
	- when both process ask for the pipe with the same name, they can read end/write end of the pipe

## Message queue, message passing

- mailbox(queue) abstraction
	- process can open mailbox at a specified location, send or recieve messages
- process can create message queue
- another process can send a message to that queue via OS
	- OS buffers messages between send and receive

### Concept

- message queue is maintained as a linked list inside the kernel
- every message has type, content, optional features like priority

#### System calls

- available to create a message queue, post and retreive messages from the queue
- blocking and non-blocking versions avilable
	- when message queue is full, writing process can block or return with an error message
	- when message queue is empty, system call to read a message can block or return empty

## Blocking and non blocking system calls(communication)

some inter-process communication system calls are blocking

- e.g. accept, reading from empty or full socket, pipe, message queue, etc,
- system call blocks process until work is done
	- e.g. when reading from socket, block until data apears on the socket
		- while blocked, cannot handle data coming on any other socket
- or they return with an error code in case of failure
- limits the number of concurrent communications a process can sustain

#### Fix techniques(for socket)

1. fork off a new child process for every connection
	- child process can dedicate to reading and writing on one connection only
2. socket can be set to non-blocking
	- process can periodically poll the socket to see if data has arrived
3. system call can be used to get notifications from the kernel on when a socket is ready for a read
	- e.g. select