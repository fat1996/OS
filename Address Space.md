Early systems:
basically just 1 running program that used up all the memory. also, the user didn't expect much from the OS, so no one complained really. everything was fine and dandy.

Next phase: multiprogramming
 -since machines were expensive, people began sharing them more efficiently.
 - the period of multiprogramming began i.e multiple processes could be run at a given time, OS could switch between them

Next phase: time sharing
- people began even more from machines. era of time sharing began.
- concept of <b>interactivity</b> became imp.
- a way to implement time sharing was to run 1 process for s little while, giving it full access to all the memory, stopping it, saving its state to some kind of disk, loading some other process's state, running it for a while, so on and so forth. pretty crude overall.
- big problem with this approach: way too slow.
- however, with new developments, multiple programs were now allowed to reside in memory <b>concurrently</b>.
- this created <b>protection</b> issues, since we don't want a process to be able to read/write some other process's memory.

<b>What is an address space?</b>
- address space is the running program's view of memory in the system.
- address space of a process contains all of the memory state of the running program. for e.g,
1. the code of the program is kept in the address space.
2. stack, which is used to keep track of the function call chain+allocating local variables, passing parameters, returning values to and from routines etc. stack is also kept here.
3. the heap, which is used for dynamically allocated, user managed memory (for e.g when you call malloc())
4. there are other things in the address space too, but we'll just remember the <b>code, stack and heap</b>.

- The code of the program lives at top of the address space. this is <b>static</b>, so we can place it at the top of the address space, since we know that it won't need any more space as the program runs.
The 2 regions of the address space that (may) grow:
1. the heap (at the top), grows downwards
2. the stack (at the bottom), grows upwards
Placing these this way allows them to grow, since they'll grow in opposite directions.
- this placement of the stack and heap is just convention, it could be changed.

- IMP: when we describe the address space, we're just describing the <b>abstraction</b> that the OS is providing to the running program.

Goals:
1. Transparency: the OS should implement virtual memory such that it's invisible to the running program. 
- the program shouldn't be aware that memory is virtualized.
- the program should behave like it has its own private physical memory.
2. Efficiency: the virtualization should be as efficient as possible, both in terms of time & space. to implement this, the OS relies on hardware features such as TLBs.
3. Protection: the OS needs to protect processes from one another. each process should run in its own <b>isolated</b> cocoon. the principle of <b>memory isolation</b> is important for building operating systems. OS's need to isolate processes from each other.

- everytime you print the address of a pointer, or any other address for that matter, it's a <b>virtual address</b>.

