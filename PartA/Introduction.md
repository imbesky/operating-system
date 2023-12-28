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

#### Memory

loads program executable(codes, datas) from disk to memory

#### CPU

initialize(set) PC and other registers to begin execution

- OS provides the process abstraction
	- process: a running program
	- 

#### External devices

read/write files from the disk


virtualization

primary way the OS does its work through a general technique
OS takes a physical resource(processor, memory, disk) and transforms it into a more general,
powerful and esy to use virtual form of itself
often refer OS = virtual machine

OS provides some interfaces(APIs) that user can call
a typical OS exports a few hundred system calls that are available to applications
= provides a standard library