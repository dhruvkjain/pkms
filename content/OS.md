[[index|Home]]

- [Physical Cores vs Logical Cores](https://techgearoid.com/articles/difference-between-physical-cores-and-logical-processors/)
- [Memory Addressing and Memory Mapped I/O](https://medium.com/@tom_84912/memory-addressing-and-memory-mapped-i-o-7b602958da94)

 [Intro to OS in C](https://kuleuven-diepenbeek.github.io/osc-course/ch1-introos/)


# Operating System Concepts

### Chapter 1

OS is a **program** that **abstracts** hardware and allows **execution** of other programs.
OS goals:
- execute user programs
- make computer convenient to use (ease of use)
- use computer hardware efficiently (resource utilization)

User <---> application programs <---> OS <---> computer hardware

middleware : software framework that provide services for application developers like database, multimedia etc.

user program <-- system call --> OS <-- interrupt --> device driver

A trap or exception is a software-generated interrupt caused either by an error or a user request.
OS is interrupt driven

Device-status table contains entry for each I/O device indicating its type, address, and state.

 ROM or EPROM contains firmware (bootstrap program), on power-up or reboot it loads OS kernel and start's it execution

Dynamic Random-access Memory (DRAM) = volatile = needs power to store data
Hard Disk Drives (HDD) = non volatile 

Direct Memory Access = Only one interrupt is generated per block, rather than one interrupt per byte

OS is Dual mode i.e. user mode (1) and kernel mode (0)

System calls usually pass control from user mode to kernel mode to execute privileged operations

A process is a program in execution
Program is a passive entity; Process is an active entity

Multiprocessor environment should ensure cache coherency i.e. ensure all processors sees a consistent view of shared memory

Spooling (Simultaneous Peripheral Operations On-Line) is an I/O management technique that temporarily stores data in secondary memory (such as a hard disk) before it is processed by a CPU or peripheral device. This is done to avoid CPU being idle while the slow I/O driver process.

### Chapter 2

Command Line Interpreter (CLI) = general term for text based interpreter  
GUI (terminal or console, sends, receives and displays text)
shell (system interpreter, does actual work)

read(), open(), write(), alarm(), pipe() etc.. are APIs that abstracts common features implemented by multiple system calls

Source code compiled into object files designed to be loaded into any physical memory location = relocatable object file
Linker combines these into single binary executable file

object files (specific to CPU architecture)
executable files (specific to CPU architecture and OS)

.c file --- Preprocessor ---> .c --- Compiler ---> assembly code (.s) --- Assembler ---> object files (.o) --- Linker ---> .exe 

statically linked libraries = linked directly with the code by compiler
dynamically linked libraries = reuse existing shared libraries, linked during runtime

Application Binary Interface (ABI) defines how compiled binaries interact

Microkernels = they moves some OS services from kernel space to the user space

Modern systems replace BIOS with Unified Extensible Firmware Interface (UEFI)

### Chapter 3

Process Control Block (PCB) = stores info like process state, counter, registers, memory etc..

![[Pasted image 20260505205801.png]]

![[Pasted image 20260505205829.png]]

Context Switch = CPU switches from one process to another

If no parent waiting (did not invoke wait()) process is a zombie
If parent terminated without invoking wait(), process is an orphan

interprocess communication (IPC) by Shared memory or Message passing

Producer Consumer Problem:
- unbounded-buffer 
- bounded-buffer

Direct Communication link = usually bidirectional and between each pair of processes there exists exactly one link; send (P, message) receive(Q, message)

Indirect Communication link = Messages are directed and received from mailboxes (ports); between each pair of processes may share multiple links

Ordinary pipes = cannot be accessed from outside the process that created it (can be used with child)

Named pipes = can be accessed by any process even without a parent-child relationship

Socket = IP + port

### Chapter 4

Concurrency = how processes are scheduled
Parallelism = how processes are allotted in a multiple cores system

Data parallelism and Task parallelism

Amdahl’s Law : S = serial part, N = number of cores
![[Pasted image 20260506030744.png]]

exec() = replace running process and all it's threads

Signals = used by kernel to notify processes that some event has occurred

user threads <--- lightweight process (LWP) ---> kernel threads


### Chapter 5

Process execution = in cycle = CPU burst followed by I/O burst

preemptive = force a process to stop execution (using interrupt) and swap away

- Throughput  = # of processes that complete their execution per time unit
- Turnaround time =  amount of time to execute a particular process
- Waiting time = amount of time a process has been waiting in the ready queue
- Response time = amount of time it takes from when a request was submitted until the first response is produced
- Burst time = amount of time a process need for it's actual full execution 

Turnaround time = Burst time + Waiting time

Scheduling Algorithms: 
- First-Come, First-Served(FCFS) Scheduling
- Shortest-Job-First (SJF) Scheduling
	- Preemptive version of SJF = Shortest Remaining Time First Scheduling
- Round Robin(RR)
	- for time quantum, q each processes take chance
- Priority Scheduling
	- to deal with Starvation = Aging = as time progresses increase the priority of the process

Determining Length of Next CPU Burst ![[Pasted image 20260506082054.png]]

Multilevel Queue
real time processes (highest priority)
system processes
interactive processes
batch processes (lowest priority)

process contention scope (PCS) = thread competing for scheduling within the process (user level threads)
system contention scope (SCS) = Kernel thread competing for scheduling with all others

Chip-multithreading (CMT) = assigns each core multiple hardware threads (Intel refers to this as hyperthreading)
this is the reason why 4 hardware core processor is said to have 8 logical cores

Soft affinity = OS attempts to keep a processes / thread running on same core
Hard affinity = OS allows process to specify which set of processors it will run

NUMA - aware = OS attempts to allot memory as close as possible to the cores processing it

Dispatch latency = time for schedule to take current process off CPU and switch to another
Interrupt latency = time from arrival of interrupt to start of routine that services interrupt

### Chapter 6

Race Condition : two processes start fork() call -> race condition on next_available_pid variable

requirements : Mutual exclusion, Progress, Bounded-waiting

Peterson’s Solution (follows all three requirements)
The n processes share two variables:
- int turn (indicates whose turn it is to enter the critical section)
- boolean flag array (n values) used to indicate if a process is ready to enter the critical section
	- flag[i] = true implies that process Pi is ready

Memory Barrier = an instruction that forces any change in memory to be propagated (made visible) to all other processors
- Strongly ordered; immediately visible
- Weakly ordered ; may not be

### Chapter 7

atomic variable that provides atomic (uninterruptible) updates

Mutex Locks = allows only one processes to access
 - acquire() lock 
 - release() lock
 - repeatedly checks "Is the lock available?" = busy waiting = spinlock

Semaphore = allows a certain number of processes to access at a time; has a waiting queue
Binary semaphore = Mutex locks

Semaphore = named and unnamed (same as pipes) 

Liveness refers to a set of properties that a system must satisfy to ensure processes make progress
- Deadlock
- Starvation = indefinite blocking = A process may never be removed from the semaphore queue in which it is suspended
- Priority Inversion = Scheduling problem when lower-priority process holds a lock needed by higher-priority process

Classical Problems of Synchronization
- Bounded-Buffer Problem
- Readers and Writers Problem
- Dining-Philosophers Problem

### Chapter 8

If graph contains no cycles => no deadlock
If graph contains a cycle =>
- if only one instance per resource type, then deadlock
- if several instances per resource type, possibility of deadlock

### Chapter 9

Register access occurs in one clock cycle, while main memory access can take many cycles, leading to a CPU "stall" 
A Cache is used between them to bridge this speed gap

Hardware Address Protection = base and limit registers check before access

Logical (Virtual) Address: Generated by the CPU.  

Physical Address: The actual address seen by the memory unit.  

Memory-Management Unit (MMU): A hardware device that maps virtual addresses to physical addresses at run time.

logical address + relocation register = base register 

External Fragmentation = needed memory exist but not contiguous
Internal Fragmentation = Allocated memory is slightly larger than requested

Physical memory is divided into fixed-sized blocks called frames 
Logical memory is divided into blocks of the same size called pages

The CPU-generated address is divided into a Page number (p) (index into the page table) and a Page offset (d) (displacement within the page)

TLB (Translation Look-aside Buffer): A small, fast hardware cache used to store recently used page-table entries

Effective Access Time (EAT): Calculated based on the hit ratio of the TLB.

### Chapter 10

Demand Paging = bring page to memory only when needed

each page table entry has a valid-invalid bit = valid if loaded in memory else invalid

page fault = process tries to access invalid page

EAT = (1 - p) * memory access + p * (page fault overhead)

Copy-on-Write (COW) = allows parent and child processes to share the same pages initially, If either process modifies a shared page, a copy of that page is made only then

Page Replacement Algorithms
- FIFO = Replaces the oldest page in memory. May suffer from Belady’s Anomaly (more frames leading to more faults)
- Optimal = Replaces the page that will not be used for the longest time. Requires future knowledge
- LRU = Replaces the page that has not been used for the longest time
- Second-Chance = FIFO variation using a reference bit to give recently used pages a "second chance"
- Counting = Keeps a count of references. Includes LFU (Least Frequently Used) and MFU (Most Frequently Used).

Allocation of Frames
Equal = Every process gets an equal share
Proportional = (size of ith process / sum of size of all processes) * total number of frames
Priority = proportional allocation based on process priorities rather than size

In global replacement, a process can take frames from others; in local replacement, it only replaces its own frames

Thrashing = a process spends more time paging than executing
prevention 
- OS tracks the Working Set Window
- WSS_i = number of pages referenced in the most recent window
- total demand $D = \sum WSS_i > \text{total memory } m$, the OS should suspend a process


### Chapter 11

NVM requires a **Flash Translation Layer (FTL)** and **garbage collection** because data cannot be overwritten in place; it must be erased in "blocks" before being rewritten in "pages."

Disk Scheduling Algorithms
- FCFS
- SCAN = The arm moves from one end to the other, servicing requests along the way. Also called the "elevator algorithm."
- C-SCAN = Similar to SCAN, but the arm returns to the beginning immediately after reaching the end. Provides more uniform wait times
- Deadline = Used in Linux; prioritizes read requests to prevent process blocking.

Swap-Space Management = Uses secondary storage as an extension of main memory (RAM)

RAID (Redundant Array of Inexpensive Disks) = increases reliability and performance through redundancy


### Chapter 12

STREAMS provide a full-duplex communication channel between a user-level process and a device
stream head (user interface), a driver end (hardware interface)












# Intro

OS is a **program** that **abstracts** hardware and allows **execution** of other programs.
It performs mainly but not definitively:
- Hardware management
- Memory management
- Task management
- CPU scheduling
- Programs execution
- Inter-Process Communication
- File management


### Architectural types of OS:
![[OS-structure2.svg]]


### System Calls
functions that abstracts a task that requires privileged mode to perform kernel level operations

system calls form API layer between user mode and kernel mode


### Linux
it is a kernel

different pieces of software + linux kernel + package management = distribution (distro)
- Linux (manages hardware, scheduling, memory, I/O = microkernel)
- GNU (every other abstractions/tools needed to create a OS)

GNOME - GNU Network Object Model Environment (Linux kernel + necessary GNU tools)

root access => sudo (Super User DO)

groups to manage access rights

shell => Command interpreter, program that reads commands and executes them

terminal => captures keystrokes and displays final results

windows => everything is a "window" object

linux => everything is a file with permission modes (read, write, execute)
- filesystem => files + their relations + their attributes or metadata
- directory => special file that can store other files (It simply references to other files it contains and does not duplicate the data itself)
- `/etc` => admin files and admin programs
- `/dev` => device files (bridge virtual OS with the physical machine)
- file permissions => rwxrwxrwx : admin/owner user -- admin group -- users
	- chmod (change mode)
	- 1 (001) : execute only, 2 (010) : write only, 4 (100) : read only
	- chmod 640 [filepath]


### File System Flavors

1. EXT (Extended File System) 