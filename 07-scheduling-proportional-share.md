# 7. Scheduling: Proportional Share
A different type of scheduler - proportional-share scheduler. Instead of optimizing for turnaround or response time, this scheduler instead try to guarantee that each job obtain a certain percentage of CPU time.

The basic idea: every so often, hold a lottery to determine which process should get to run next; processes that should run more often should be given more chances to win the lottery.

Tickets are used to represent the share of a resource that a process should receive. The percent of tickets that a process has represents its share of the system resource.

One of the most beautiful aspects of lottery scheduling is its use of randomness. When you have to make a decision, using such a randomized approach is often a robust and simple way of doing so.

Ticket currency: a user with a set of tickets can allocate tickets among their own jobs in whatever currency they would like; the system then automatically converts said currency into the correct global value.

Ticket transfer: a process can temporarily hand off its tickets to another process. This is especially useful in a client/server setting, where a client process sends a message to a server asking it to do some work on the client's behalf. To speed up the work, the client can pass the tickets to the server and thus try to maximize the performance of the server while the server is handling the client's request. When finished, the server then transfers the tickets back to the client and all is as before.

Ticket inflation: a process can temporarily raise or lower the number of tickets it owns. Can be applied in an environment where a group of processes trust one another; if any one process knows it needs more CPU time, it can boost its ticket value as a way to reflect that need to the system, all without communicating with any other processes.

Lottery makes it much easier to incorporate new processes in a sensible manner.

The current Linux approach achieves similar goals in an alternate manner. The scheduler, entitled the `Completely Fair Scheduler (CFS)`, implements fair-share scheduling, but does so in a highly efficient and scalable manner. It does so through a simple counting-based technique known as virtual runtime (vruntime).

As each process runs, it accumulates vruntime. CFS uses various control parameters. The first is sched latency. CFS uses this value to determine how long one process should run before considering a switch (effectively determining its time slice but in a dynamic fashion). A typical sched latency value is 48 (milliseconds); CFS divides this value by the number (n) of processes running on the CPU to determine the time slice for a process, and thus ensures that over this period of time, CFS will be completely fair. CFS also have another parameter, min granularity, which is usually set to a value like 6 ms. CFS will never set the time slice of a process to less than this value, ensuring that not too much time is spent in scheduling overhead. 

CFS also enables controls over process priority, enabling users or administrators to give some processes a higher share of the CPU. It does this through a classic UNIX mechanism known as the nice level of a process. When you're too nice, you just don't get as much (scheduling) attention, alas.

## Read-black trees
One aspect of efficiency: `when the scheduler has to find the next job to run, it should do so as quickly as possible`. Simple data structures like lists don't scale: modern systems sometimes are comprised of 1000s of processes, and thus searching through a long-list every so many milliseconds is wasteful. CFS addresses this by keeping processes in a `red-black tree`. 

A red-black tree is a type of balanced tree - balanced trees maintain low depths, and thus ensure that operations are logarithmic in time. CFS only keeps running/runnable processes in there. If a process goes to sleep (by waiting on an I/O to complete, or for a network packet to arrive), it is removed from this tree.

## Dealing with I/O and sleeping processes
Imagine two processes, A and B, A runs continuously, B has gone to sleep for a long period of time (10 seconds). When B wakes up, its vruntime will be 10 seconds behind A's, and thus, B will now monopolize the CPU for the next 10 seconds while it catches up, starving A.

CFS handles this by altering the vruntime of a job when it wakes up. Specifically, CFS sets the vruntime of that job to the minimum value found in the tree. In this way, CFS avoids starvation, but not without a cost: jobs that sleep for short periods of time frequently do not ever get their fair share of the CPU. 

## Summary
CFS is a bit like weighted round-robin with dynamic time slices, but built to scale and perform well under load. It is the most widely used fair-share scheduler today.

In a virtualized data center, or cloud, where you might like to assign 1/4 of your CPU cycles to the Windows VM, and the rest to your base Linux installation, proportional sharing can be simple and effective.

