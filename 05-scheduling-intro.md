## 5. Scheduling: Introduction
workload: all the processes running in the system.

The turnaround time of a job: the time when the job completes minus the time when the job arrived in the system.

The most basic scheduling algorithm is "First In, First Out (FIFO) scheduling". Advantages: Simple & easy to implement. Disadvantages: Convoy effect. Many relatively short potential consumers get queued behind a heavyweight resource consumer. 

Shortest Job First (SJF): it runs the shortest job first, then the next shortest, and so on. Disadvantages: if shorter job arrived slightly after the heavy job, nothing will be improved. 

Shortest Time-to-Completion First (STCF): Any time a new job enters the system, the STCF scheduler determines which of the remaining jobs (including the new job) has the least time left, and schedules that one.

For many early batch computing systems, these types of scheduling algorithms made some sense. However, the introduction of time-shared machines changed all that. Now users would sit at a terminal and demand interactive performance from the system. So a new metric was born: response time.

Response time: the time from when the job arrives in a system to the first time it is scheduled. 

STCF and related disciplines are not particularly good for response time.

Round-Robin (RR) scheduling: runs a job for a time slice (a scheduling quantum) and then switches to the next job in the run queue. It repeatedly does so until the jobs are finished. Also called time-slicing.

Making the time slice too short is problematic: the cost of context switching will dominate overall performance. Thus, deciding on the length of the time slice presents a trade-off, making it long enough to amortize the cost of switching without making it so long that the system is no longer responsive. When programs run, they build up a great deal of state in CPU caches, TLBs, branch predictors, and other on-chip hardware. Switching to another job causes this state to be flushed and new state relevant to the currently-running job to be brought in, which may exact a noticeable performance cost. 

RR is indeed one of the worst policies if turnaround time is our metric. Because turnaround time only cares about when jobs finish, RR is nearly pessimal, even worse than simple FIFO in many cases. More generally, any policy (such as RR) that is fair, i.e., that evenly divides the CPU among active processes on a small time scale, will perform poorly on metrics such as turnaround time. 

A scheduler has a decision to make when a job initiates an I/O request, because the currently-running job won't be using the CPU during the I/O; it is blocked waiting for I/O completion. The scheduler should probably schedule another job on the CPU at that time.

The scheduler also has to make a decision when the I/O completes. When that occurs, an interrupt is raised, and the OS runs and moves the process that issued the I/O from blocked back to the ready state.

While those interactive jobs are performing I/O, other CPU-intensive jobs run, thus better utilizing the processor.

