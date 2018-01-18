<b>Scheduling policies:</b>
Before getting into the range of possible policies, let us first make a number of simplifying assumptions about the processes running in the system, sometimes collectively called the workload.

<b>Metrics used:</b>

	1. Metric we use: <b>turnaround time</b>
	The turnaround time of a job is defined as the time at which the job completes minus the time at which the job arrived in the system. 

	turnaround_time = completion_time − arrival_time
	- this is a performance metric

	2. A new metric aside from turnaround time: <b>response time</b>
	We define response time as the time from when the job arrives in a system to the first time it is scheduled.

	response_time = first_time_it's_scheduled − arrival_time 

<b>Different algorithms that are employed for scheduling processes:</b>
	
	1. FIFO
	- <b>convoy effect:</b> where a number of relatively-short potential consumers of a resource get queued behind a heavyweight resource consumer.

	2. SJF: Shortest Job First
	- SJF is indeed an optimal scheduling algorithm

	3. STCF: Shortest time to completion first anytime a new job enters the system, the STCF scheduler determines which of the remaining jobs (including the new job) has the least time left, and schedules that one.

	4. anothing scheduling algorithm: Round Robin
	The basic idea is simple: instead of running jobs to completion, RR runs a job for a time slice (sometimes called a scheduling quantum) and then switches to the next job in the run queue. It repeatedly does so until the jobs are finished. For this reason, RR is sometimes called <b>time-slicing.</b>

	5. Most well known approach to scheduling: <b>The Multi-Level Feedback Queue(MLFQ)</b>
	Basic rules of MLFQ:
	- In our treatment, the MLFQ has a number of distinct queues, each assigned a different priority level. 
	- At any given time, a job that is ready to run is on a single queue. 
	- MLFQ uses priorities to decide which job should run at a given time: a job with higher priority (i.e., a job on a higher queue) is chosen to run.
	- Of course, more than one job may be on a given queue, and thus have the same priority. In this case, we will just use round-robin scheduling among those jobs.
	- Thus, we arrive at the first two basic rules for MLFQ:
	• Rule 1: If Priority(A) > Priority(B), A runs (B doesn’t).
	• Rule 2: If Priority(A) = Priority(B), A & B run in RR.

	- The key to MLFQ scheduling therefore lies in how the scheduler sets priorities. 
	- Rather than giving a fixed priority to each job, MLFQ varies the priority of a job based on its observed behavior. If, for example, a job repeatedly relinquishes the CPU while waiting for input from the keyboard, MLFQ will keep its priority high, as this is how an interactive
	process might behave. If, instead, a job uses the CPU intensively for long periods of time, MLFQ will reduce its priority. In this way, MLFQ will try to learn about processes as they run, and thus use the history of the job to
	predict its future behavior.

	6. Another scheduler is the proportional-share scheduler, also sometimes referred to as a fair-share scheduler. Proportional-share is based around a simple concept: instead of optimizing for turnaround or response time, a scheduler might instead try to guarantee that each job obtain a certain percentage of CPU time.