# 8. Multiprocessor Scheduling (Advanced)
Multicore processor has many multiple CPU cores packed onto a single chip. 

This creates many difficulties, eg, typical application (i.e., some C program) only uses a single CPU; adding more CPUs does not make that single application run faster. So you will have to rewrite your application to run in parallel, perhaps using threads. 

How to schedule jobs on multiple CPUs?

In a system with a single CPU, there are a hierarchy of hardware caches, that in general help the processor run programs faster. Disk, in contrast, holds all of the data, but access to this larger memory is slower. 

Caches are based on the notion of locality, of which there are two kinds: 
- temporal locality. When a piece of data is accessed, it is likely to be accessed again in the near future
- spatial locality. If a program accesses a data item at address x, it is likely to access data items near x as well

Caching with multiple CPUs is much more complicated. For example, the problem of cache coherence - cpu 1 reads value x from disk and increment x, and need to write it back to disk. Since writing to disk is slow, so cpu 1 decided to batch them later. Then OS stopped the running of this code on cpu1, and move it to cpu 2, who again needs to read x and increment, cpu2 went to disk and grabbed a stale x value. 

`Bus snooping` is one way to resolve this. Each cache pays attention to memory updates, by observing the bus that connects them to disk. When a CPU sees an update for a data item that it holds in its cache, it will notice the change, and either invalidate its copy of this data item, or update it. 

## Synchronization
When accessing/updating shared data items or structures across CPUs, locks should likely be used to guarantee correctness. 

## Cache affinity
A process, when run on a particular CPU, builds up a fair bit of state in the CPU cache. The next time the process runs, it is often advantageous to run it on the same CPU, as it will run faster if some of its state is already present in that CPU cache.

So a multiprocessor scheduler should consider cache affinity, when making its scheduling decisions. 

## Single-queue multiprocessor scheduling (SQMS)
Reuse the basic framework for single processor scheduling, by putting all jobs that need to be scheduled into a single queue. 

SQMS has shortcomings:
1. A lack of scalability. To ensure the scheduler works correctly on multiple CPUs, the developers will have inserted some form of locking into the code, which greatly reduce performance. 
2. Cache affinity. Because each CPU simply picks the next job to run from the globally-shared queue, each job ends up bouncing around from CPU to CPU. 

## Multi-queue multiprocessor scheduling (MQMS)
Consists of multiple scheduling queues. Each queue will likely follow a particular scheduling discipline, such as round robin. 

MQMS is inherently more scalable. In addition, MQMS intrinsically provides cache affinity - jobs stay on the same CPU,.

However, fundamental in the multi-queue based approach: load imbalance. Some CPUs may have higher load than others. Solution: Move jobs around from on CPU to another(migration).

How should the system do migration? Work stealing - a queue that is low on jobs will occasionally peek at another queue, to see how full it is. If the target queue is more full than the source queue, the source will "steal" some jobs from the target to help balance load. 

