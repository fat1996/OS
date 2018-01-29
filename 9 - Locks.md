lock_t mutex; // some globally-allocated lock ’mutex’

lock(&mutex);
balance = balance + 1;
unlock(&mutex);

A lock is just a variable, and thus to use one, you must declare a lock
variable of some kind (such as mutex above). This lock variable (or just
“lock” for short) holds the state of the lock at any instant in time. It is either
available (or unlocked or free) and thus no thread holds the lock, or
acquired (or locked or held), and thus exactly one thread holds the lock
and presumably is in a critical section.

The semantics of the lock() and unlock() routines are simple. Calling
the routine lock() tries to acquire the lock; if no other thread holds
the lock (i.e., it is free), the thread will acquire the lock and enter the critical
section; this thread is sometimes said to be the owner of the lock. If
another thread then calls lock() on that same lock variable (mutex in
this example), it will not return while the lock is held by another thread;
in this way, other threads are prevented from entering the critical section
while the first thread that holds the lock is in there.

Once the owner of the lock calls unlock(), the lock is now available
(free) again. If no other threads are waiting for the lock (i.e., no other
thread has called lock() and is stuck therein), the state of the lock is
simply changed to free. If there are waiting threads (stuck in lock()),
one of them will (eventually) notice (or be informed of) this change of the
lock’s state, acquire the lock, and enter the critical section.

Locks yield some of that control back to the programmer; by putting
a lock around a section of code, the programmer can guarantee that no
more than a single thread can ever be active within that code. 

Before building any locks, we should first understand what our goals
are, and thus we ask how to evaluate the efficacy of a particular lock
implementation. To evaluate whether a lock works (and works well), we
should first establish some basic criteria. The first is whetherthe lock does
its basic task, which is to provide mutual exclusion. Basically, does the
lock work, preventing multiple threads from entering a critical section?
The second is fairness. Does each thread contending for the lock get
a fair shot at acquiring it once it is free? Another way to look at this is
by examining the more extreme case: does any thread contending for the
lock starve while doing so, thus never obtaining it?
The final criterion is performance, specifically the time overheads added
by using the lock. There are a few different cases that are worth considering
here. One is the case of no contention; when a single thread
is running and grabs and releases the lock, what is the overhead of doing
so? Another is the case where multiple threads are contending for
the lock on a single CPU; in this case, are there performance concerns? Finally,
how does the lock perform when there are multiple CPUs involved,
and threads on each contending for the lock? By comparing these different
scenarios, we can better understand the performance impact of using
various locking techniques, as described below.

Lock based concurrent data structures:
Adding locks to a data structure to make it usable
by threads makes the structure thread safe

Threads vs Processes:
a thread is like a worker in a toy shop because its an active entity, it works simultaneoulsy with other workers, and requires coordination(sharing of resources).

