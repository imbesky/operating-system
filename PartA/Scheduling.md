# Scheduling

- OS scheduler employ high-level policy
- framework should be developed thinking about scheduling policies

## Scheduling metrics

metric: method to measure something

- fairness; measured by Jain's fairness index
- trade off:
	- turnaround time <-> response time
	- fairness <-> response time

### Turnaround time

`T(turnaround) = T(completion) - T(arrival)`

- performance metric

### Response time

`T(response) = T(first run) - T(arrival)`

## Scheduling policy

algorithms for scheduling

- = discipline

### FIFO

= FCFS, First Come First Served

- if running time of earlier process is formidably long, the total(and everage) turnaround time will rapidly increase

#### Convoy effect

number of relatively-short potential consumers of a resource get queued behind a heavyweight resource consumer

### SJF

= Shortest Job First

- optimal for turnaround time; when all jobs arrive at the same time
- bad for response time
- when the process with long running time arrives earlier, the total(and everage) turnaround time will rapidly increase

### STCF

= Shortest Time-to-Completion First = Preemptive Shortest Job First(PSJF)

- SJF + preemption
	- preempt the job that has shorter running time
	- any time a new job enters system, scheduler determines which of the remaining jobs has the least time left and schedules that one
- optimal for turnaround time
- bad for response time

### RR

= Round Robin = time-slicing

- runs a job for a time slice(= scheduling quantum) and switches to the next job in the run queue
- optimal for response time
- worst for turnaround time, fairness

#### Time slice

- length of time slice must be a multiple of the timer-interrupt period
- shorter time slice, better performance
- too short time slice increases cost of context switching and dominate overall performance
	- 번외: cost of context switch
		- OS actions like saving and retoring registers
		- flushing and bringing state in CPU caches, TLBs, branch predictors, and other on-chip hardware

### 번외: Preemptive scheduler

Preemptive: 우선권이 있는

- non-preemptive
	- used in old days of batch computing
	- systems run each job to completion before considering whether to run a new job
	- ex: FIFO, SJF
- preemptive
	- used in all modern schedulers
	- willing to stop one process from running  in order to run another
	- imply that the scheduler employs the mechanisms(like context switch)
	- ex: STCF

## CPU sharing

### Incorporate I/O

- when I/O is sent to a hard disk drive
	- currently running job is blocked and won't be using CPU
	- scheduler probably schedule another job on the CPU
- when I/O complete schduler should make a decision
	- run the job that issued I/O or not

#### Overlap

to use CPU efficiently, treat sub-job of job that issues I/O independently

- when I/O is issued, run other job; interactive

## Muti-level feedback queue(MLFQ)

fundamental problem MLFQ tries to address;
1. optimize turnaround time
	- done by running shorter jobs first
	- problem: general purpose OS usually knows very little about the lenght of each job
2. minimize response time
	- problem: algorithms that reduce response time is terrible for turnaround time

so the crux: how to schedule without perfect knowledge
- MLFQ learns form past to predict future

### Basic rules

#### Assumptions

- number of distinct queues, assigned in a different priority level
	- MLFQ vaies priority based on observed behavior
		- high: job that repeatedly relinquishes CPU
		- low: jog that uses CPU for intensively long period
- a job ready to run is on single queue

#### Rules

1. if Priority(A) > Priority(B): run A
2. if Priority(A) = Priority(B): run A&B in RR

### Changing priority

- allotment(할당): amount of time a job can spend at a given priority level before the scheduler reduces its priority

#### Assumptions

- workload: mix of short-running(interactive) jobs and loger-running(CPU-bound) jobs
- at first, allotment is equal to a single time slice

#### Rules

3. when job enters system, it is placed at the highest priority(topmost queue)
	- because OS not knows the length of job, it first assumes that the job might be a short one and give high priority

4.
	1. if job uses its allotment while running, its priority is reduced; moves down one queue
	2. if job gives up the CPU before the allotment is up, its priority level stays at the same level; reset allotment
		- an interactive job doing a lot if I/O will relinquish CPU requently; simply keep it at the same level to not to penlize it

### Problems

1. starvation
	- having many interactive jobs lead to comsumption of all CPU time
	- long-running jobs cannot receive CPU time; they starve
2. game the scheduler
	- user can trick scheduler bothering fair share of the resource
	- ex: before the allotment use, issue an I/O to preserve priority of the job; gain higher percentage of CPU time
3. program can change behavior on time
	- CPU-bound(loger running) may transition to a phase of interactivity(short running)

### Priority boost

#### Rules

5. after some time period, move all jobs in the system to highest priority(topmost queue)
	- to avoid starvation and program behavior changing
	- periodically boost the priority of all jobs in the system; can be implemented in other way

#### Time period

- long time period will make long running jobs starve
- short time period will disturb interactive jobs from getting proper share of CPU

1. let system administrator to find right value
2. automatic method based on machine running

### Better accounting

solution for gaming the scheduler: accounting CPU time at each level of MLFQ

#### Rules

edit rule 4

4. once a job uses up its time allotment(regardless of how many times it relinquish CPU), its priority is reduced

### Tuning MLFQ

Parameterizing things like number of queues, length of slice by per queue, ammount of allotment, period of priority boost, etc.

- to satisfactory balance, exeperience with workloads and subsequent tuning of scheduler needed
- most MLFQ allow varying time slice length accross different queues
	- high priority, short time slice; as job with high priority is interactive, quick alternating make sense