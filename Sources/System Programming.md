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
