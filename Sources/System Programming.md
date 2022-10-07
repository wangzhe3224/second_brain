#linux 

[[API]] makes sure source compatibility, the user of the API will successfully compile with the implementation of the API.
[[ABI]]  makes sure binary compatibility, it makes sure that a piece of object code will function on any system with the same ABI without recompilation. 

IPC, inter-process communication:
- pipes
- named pipes
- semaphores
- message queue
- share memory
- futexes ( fast userspace mutex )

## Process

In Linux, loading program into memory is separated from creating a new process. 
- exec, loading new program into process memory
	- execl, 
- fork, create a new process
	- child process has following differences than parent process:
		- pid
		- resource statistics are set to 0
		- pending signals are cleared
		- file locks are not inherited 
- Copy on write, COW
	- has hardware support, MMUs
	- efficient forking
- Terminate process
	- exit()
	- atexit()
	- SIGCHLD
		- when process terminated, kernel send SIGHILD to parent process
- zombie process
	- a process is terminated but not waited by any other process yet
- Process group
	- A collection of processes which are close related to a job
	- identified by `pgid`
	- we can signal the whole process group
- A Session is a collection of process groups
	- `setsid` creates a new session  
- Deamons
	- a process runs at background without any controlling terminal
	- create a deamons
		- `fork` a new process
		- in the parent call `exit`
		- `setsid()` gives the deamon a new process group and session
		- change the working directory to root
		- close all file descritors
		- open fd : 0, 1, 2 and direct them all to `/dev/null`
	- or just use `deamon()` to create (above operations)

## Process Scheduling

Linux has a preemptive process scheduler with O(1) time complexity. The scheduler is self-contained, in `kernel/sched.c`. 

- timeslice
	- time that a process can run before preempted
	- tricky to decide, too long or too short
	- linux has a dynamic timeslice allocation policy
- threading
	- units of execution within in a single process
	- Linux sees threading just as process but with some shared resources
- yielding the processor
	- `sched_yield` to yield the execution time back to kernel 
- process priority
	- nice value
		- `[-19, 20]` , the smaller the higher priority. 
		- `int nice(int inc)` to set nice value of a process
	- `getpriority` and `setpriority` is preferred 
- processor affinity
	- likelihood of a process to be scheduled consistently on the same processor
	- cache cost to switch processor
- Resource limit
	- 

## File and Directory Management

- File
	- File is referenced in Linux as `inode`, which stores metadata of a file, such as permissions, last access timestamp, etc.
	- ls -i will show inode number
	- `stat` libraries provides c function to operate on file and directories. 
	- we can store extra meta data to inode structure.
- Directory
	- is just collections of directory and inode mappings
- Links
	- each name to inode mapping is called a link
	- hard link: file can have 0 or 1 or more hard link
	- symlinks, or soft links: 
- Device Nodes
	- files that allow application to interact with device drivers
		- major number and minor number to decide which version of driver code to load
		- `/dev/null` is a special device that all user can read and write to
		- `/dev/zero`, if read from, get infinite stream of null bytes
		- `/dev/full` , if read form, get infinite stream of `\0` char
		- `/dev/random`, reads random numbers
	- out of band communication
		- sending special signals to device, such as eject cd rom... 
- Monitoring File Events
	- `inotify` to monitor files, by inotify instance. 

## Memory Management 

- Page
	- 4 or 8 bytes
- Copy on Write, COW
	- when want to write, kernel create a copy of the page
- Memory region
	- text segment, program code
	- stack
	- data segment, or heap
	- bss segment, global variables
	- mapped files
		- `/proc/self/maps` 
		- linked libraries, shared libraries, etc...
- Dynamic Memory Allocation
	- `malloc`
	- `calloc`, allocate memory and fill 0
	- `realloc` 
	- `free`, free the memory pointed by 
	- `mmap`, memory mapping of files
	- Debug Memory Allocations `mallinfo` 
- Stacked Based Allocation
	- `alloca` allocation on stack, no need to free them
- Manipulating Memory
	- `memset`
	- `memcmp`
	- `memmove`
	- `memcpy`
	- `memchr`

## Signals

Signals are software interrupts that provide a mechanism for handling asynchronous events. 

Signals have very precise lifecycle:
- raised/sent/generated
- store (with the kernel)
- deliver
- handle

What we do with signals?
- ignore
- catch
- default action

Notes:
- When the kernel raises a signal, a process can be executing code anywhere. 
	- reentrancy function
		- not manipulating static data
		- only manipulating stack-allocated data or data from caller
		- not invoking nonreentrant function

## Time

Kernel measures time in:
- Wall time or real time
	- actual time and date in real world
- process time
	- time process has consumed, in user-space and kernel space
- monotonic time
	- cannot be changed, like wall time could be set by user 