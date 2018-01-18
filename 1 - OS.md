<b>Monolithic OS:</b>

Earliest OS's had this design.
Everything that a particular application or hardware will require was already a part of the operating system.

	Pros:
	1. everything is included.
	2. some possibilities for some compile time optimizations.

	Cons:
	1. Way too large
	2. large size meant large memory requirement
	3. this can impact the performance of various applications.

<b>Modular OS:</b>
1. More common approach today
2. has basic services and API's
3. but things can be added
4. good for customization

	Pros:
	1. less resource intensive
	2. maintainable

	Cons:
	1. reduced opportunities for optimizations

<b>Conclusion: Definitely</b> much better than monolithic approach

<b>All about processes:</b>
	- A process is an instance of an executing program.
	- Visual metaphor: its like an order of toys submitted to a toy shop. building these toys will require some specific items/hardware like sewing machines, glue guns etc.
	- a process has a state of execution which is specified by a program counter, stack etc. the OS controls which process gets executed when, which process uses which part of what hardware etc, control how different processes are executed concurrently.
	- One of the roles of the OS is to manage hardware on behalf of the applications. once an applications is launched, its loaded in memory, and it starts executing.
	- application in itself is a static entity when its just a program on disk or in flash memory.
	- a process is an active entity.
	- multiple processes can be executed concurrently. for eg, multiple instances of the word editor represent concurrent processes.

From the textbook:
- <b>The problem:</b> how do we provide the illusion of many CPU's when in reality, there's only 1?
Answer: by virtualizing the CPU.
it runs 1 process, stops it, runs another process, stops it, so on and so forth. by doing so, it provides the illusion of many CPU's.
This is called <b>time sharing</b> of the CPU, which allows users to run as many concurrent processes as possible.

To understand what constitutes a process, we thus have to understand its machine state: what a program can read or update when it is running.
At any given time, what parts of the machine are important to the execution of this program?
1. One obvious component of machine state that comprises a process is its memory. Instructions lie in memory; the data that the running program reads and writes sits in memory as well. Thus the memory that the process can address (called its <b>address space</b>) is part of the process.
2. Also part of the process’s machine state are registers; many instructions explicitly read or update registers and thus clearly they are important to the execution of the process.
3. There are some particularly special registers that form part of this machine state. For example, the program counter (PC) (sometimes called the instruction pointer or IP) tells us which instruction of the program is currently being executed; similarly a stack pointer and associated frame pointer are used to manage the stack for function parameters, local
variables, and return addresses.

<b>How it works:</b>
	1. The first thing that the OS must do to run a program is to load its code and any static data (e.g., initialized variables) into memory, into the address space of the process. 

	2. Programs initially reside on disk (or, in some modern systems, flash-based SSDs) in some kind of executable format; thus, the process of loading a program and static data into memory requires the OS to read those bytes from disk and place them in memory
	somewhere.

	3. Once the code and static data are loaded into memory, there are a few other things the OS needs to do before running the process. Some memory must be allocated for the program’s run-time stack (or just stack). As you should likely already know, C programs use the stack for local variables, function parameters, and return addresses; the OS allocates this memory and gives it to the process. 

	4. The OS will also likely initialize the stack with arguments; specifically, it will fill in the parameters to the main() function, i.e., argc and the argv array.

	5. The OS may also allocate some memory for the program’s heap. In C programs, the heap is used for explicitly requested dynamically-allocated data; programs request such space by calling malloc() and free it explicitly by calling free(). The heap is needed for data structures such as linked lists, hash tables, trees, and other interesting data structures. The
	heap will be small at first; as the program runs, and requests more memory via the malloc() library API, the OS may get involved and allocate more memory to the process to help satisfy such calls.

	6. The OS will also do some other initialization tasks, particularly as related
	to input/output (I/O). For example, in UNIX systems, each process
	by default has three open file descriptors, for standard input, output, and
	error; these descriptors let programs easily read input from the terminal
	as well as print output to the screen. 

	7. The OS has now (finally) set the stage for program execution. It thus has one last
	task: to start the program running at the entry point, namely main(). By
	jumping to the main() routine, the OS transfers control of the CPU to the
	newly-created process, and thus the program begins its execution.

The different states a process can be in:
	1. Running: In the running state, a process is running on a processor.
	This means it is executing instructions.

	2. Ready: In the ready state, a process is ready to run but for some
	reason the OS has chosen not to run it at this given moment.

	3. Blocked: In the blocked state, a process has performed some kind
	of operation that makes it not ready to run until some other event
	takes place. A common example: when a process initiates an I/O
	request to a disk, it becomes blocked and thus some other process
	can use the processor.

Being moved from ready to running means the process has been <b>scheduled</b>;
being moved from running to ready means the process has been <b>descheduled</b>.

<b>Context switch</b>: stopping one running process temporarily and resuming (or starting) another.

<b>Process creation in UNIX systems:</b>
1. processes are created with a pair of system calls: fork() and exec().

fork() system call:
- used to create a new process.
See example program created: fork.c

Explanation of what's happening in fork.c:
When it first started running, the process prints out a hello world message; included
in that message is its <b>process identifier</b>, also known as a <b>PID</b>. The
process has a PID of 29146; in UNIX systems, the PID is used to name
the process if one wants to do something with the process, such as (for
example) stop it from running. 

<b>The CPU scheduler</b>, determines which process runs at a given moment in time; because the scheduler is complex, we cannot usually make strong assumptions about what it will choose to do, and hence which process will run first.

<b>wait() system call:</b>
Sometimes, as it turns out, it is quite useful for a parent to wait for a child process to finish what it has been doing. This task is accomplished with the wait() system call.
Adding a wait() call to the code above makes the output deterministic.

<b>Different processor modes:</b>
	1. user mode: code that runs in user mode is restricted in what it
	can do. For example, when running in user mode, a process can’t issue
	I/O requests; doing so would result in the processor raising an exception;
	the OS would then likely kill the process.
	2. kernel mode: which the operating system (or kernel) runs in. In this mode, code that runs can do what it likes, including privileged operations such as issuing I/O requests and executing all types of restricted instructions.
	3. system call: what should a user process do when it wishes to perform some kind of privileged operation, such as reading from disk? To enable this, virtually all modern hardware provides the ability for user programs to perform a system call.

<b>Executing a system call:</b>
To execute a system call, a program must execute a special <b>trap</b> instruction.
This instruction simultaneously jumps into the kernel and raises the
privilege level to kernel mode; once in the kernel, the system can now perform
whatever privileged operations are needed (if allowed), and thus do
the required work for the calling process. 
When finished, the OS calls a special <b>return-from-trap</b> instruction, which returns
into the calling user program while simultaneously reducing the privilege level back to user mode.

When a process gets stuck in an infinite loop it resorts to the age-old solution to all problems in computer systems: <b>reboot the machine.</b>

