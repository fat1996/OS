Analogy from book:
1. Imagine a lot of peaches on a table, and a lot of people who want to eat them.
2. Each eater identifies a peach visually, and they then try to grab it and eat it.
3. The problem here is that you could visually like a peach, but by the time you get to the table to get the peach, someone else could've done the same, and already eaten it. 

Possible solutions:
1. we could get everyone to make a line, and grab peaches that way.

How does this apply to computers:
Well,
as it turns out, there are certain types of programs that we call multi-threaded
applications; each thread is kind of like an independent agent running around
in this program, doing things on the program’s behalf. But these threads access
memory, and for them, each spot of memory is kind of like one of those peaches. If
we don’t coordinate access to memory between threads, the program won’t work
as expected.

Multi-Threaded Address Space:
Instead of a single stack in the address space, there will be one per
thread.
Thus, any stack-allocated variables, parameters, return
values, and other things that we put on the stack will be placed in
what is sometimes called thread-local storage, i.e., the stack of the relevant
thread.

Rationale for using threads:
1.  parallelism. Imagine you are writing a program
that performs operations on very large arrays, for example, adding
two large arrays together, or incrementing the value of each element in
the array by some amount. If you are running on just a single processor,
the task is straightforward: just perform each operation and be done.
However, if you are executing the program on a system with multiple processors, you have the potential of speeding up this process considerably
by using the processors to each perform a portion of the work. The
task of transforming your standard single-threaded program into a program
that does this sort of work on multiple CPUs is called parallelization,
and using a thread per CPU to do this work is a natural and typical
way to make programs run faster on modern hardware.
2. The second reason is a bit more subtle: to avoid blocking program
progress due to slow I/O. Imagine that you are writing a program that
performs different types of I/O: either waiting to send or receive a message,
for an explicit disk I/O to complete, or even (implicitly) for a page
fault to finish. Instead of waiting, your program may wish to do something
else, including utilizing the CPU to perform computation, or even
issuing further I/O requests. Using threads is a natural way to avoid
getting stuck; while one thread in your program waits (i.e., is blocked
waiting for I/O), the CPU scheduler can switch to other threads, which
are ready to run and do something useful.

A critical section is a piece of code that accesses a shared resource,
usually a variable or data structure.
• A race condition arises if multiple threads of execution enter the
critical section at roughly the same time; both attempt to update
the shared data structure, leading to a surprising (and perhaps undesirable)
outcome.
• An indeterminate program consists of one or more race conditions;
the output of the program varies from run to run, depending on
which threads ran when. The outcome is thus not deterministic,
something we usually expect from computer systems.
• To avoid these problems, threads should use some kind of mutual
exclusion primitives; doing so guarantees that only a single thread
ever enters a critical section, thus avoiding races, and resulting in
deterministic program outputs.