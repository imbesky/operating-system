# Operating System: Processes

## What is OS

middleware between user programs and system hardware

- = resource manager
- manages resources, hardware(CPU, memory, disk, I/O devices)
- body of software
	- reponsible for making easy to run programs
	- allow programs to share memory
	- enable programs to interact with devices

## What happens when we run a program

### Background

- complier translate high level program into executable

#### Executable

the `exe` contains
1. instructions that the CPU can understand 
2. data of the program

- all numbered with address
- instructions run on CPU
	- hardware implements an ISA; Instruction Set Architecture
	- every CPU has ISA
- CPU consists of a few registers
	- program counter(PC): pointer to current instruction
	- operands of instructions, memory addresses

### Von Neumann model of compution

#### To run exe, CPU

processeor; for many millions~billions of times every second
- fetches an instruction pointed at by PC(program counter) from memory
- loads data required by the instructions into registers
- decodes and executes(operate, access memory, check condition, jump, etc.) the instruction
- stores result to memory

when it is done with this instruction, the processor moves on to the next instruction

#### Cache

most recently used instructions and data are in CPU caches for faster access

## Role of OS

managing resources like hardware

it allows many programs
- to run thus sharing CPU
- concurrently accss thier own instructions and data thus sharing memory
- to access devices thus sharing disks and so forth

### Hardware management

#### CPU

initialize(set) PC and other registers to begin execution

- OS provides the process abstraction
	- process: a running program
	- OS creates and manage processes

- each process has the illusion of having complete CPU
	- i.e. OS virtualizes CPU

- time-shares CPU between processes

- enables coordination between processes

#### Memory

loads program executable(codes, datas) from disk to memory

- manages memory of process: code, data, stack, heap, etc

- each process thinks it has a dedicated memory space for itself
	- numbers code and data starting from 0
	- the virtual addresses

- OS abstracts out the details of actual placement in memory
	- by translating from virtual addresses to actual physical addresses
	- 연속된 주소에 저장되는 게 아니라 나누어서 저장될 수 있다는 말

#### External devices

read/write files from the disk

- OS has code to manage disk, network card and other external devices
	- it is device drivers

- device drivers talks the language of the hardware devices
	- fetch data from a file; issues instructions to devices
	- responds to interrupt events from devices; ex. pressing keyboard, clicking mouse

- persistent data organized as a filesystem on disk

### Virtualization

primary way the OS does its work through a general technique
OS takes a physical resource(processor, memory, disk) and transforms it into a more general,
powerful and esy to use virtual form of itself
often refer OS = virtual machine

OS provides some interfaces(APIs) that user can call
a typical OS exports a few hundred system calls that are available to applications
= provides a standard library

## Design goals of an OS

- Convenience
	- abstraction of hardware resources for user programs
- Efficiency
	- of usage of CPU, memory, etc.
- Isolation
	- between multiple processes

## History of OS

1. start as a library
	- to provide common functionality across programs
2. evolved from procedure call to system call
	- call occurrs after invoking OS functions
	- system call is made to run OS code; CPU executes at a higher privilege level
3. evolved from running a single program to multiple processes concurrently