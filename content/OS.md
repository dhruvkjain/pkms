[[index|Home]]

- [Physical Cores vs Logical Cores](https://techgearoid.com/articles/difference-between-physical-cores-and-logical-processors/)
- [Memory Addressing and Memory Mapped I/O](https://medium.com/@tom_84912/memory-addressing-and-memory-mapped-i-o-7b602958da94)

 [Intro to OS in C](https://kuleuven-diepenbeek.github.io/osc-course/ch1-introos/)

# Intro

OS is the **program** that **abstracts** hardware and allows **execution** of other programs.
It performs mainly but not definetively:
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