# 20. Concurrency: An Introduction
Thread. A multi-threaded program has more than one point of execution (i.e., multiple program counters, each of which is being fetched and executed from). Each thread is like a separate process, except they share the same address space, and thus can access the same data.

The state of a single thread is very similar to that of a process. It has a program counter (PC), that tracks where the program is fetching instructions from. Each thread has its own private set of registers it uses for computation; thus, if there are two threads that are running on a single processor, when switching from running one (T1) to running the other (T2), a context switch must take place.

With processes, we saved state to a process control block (PCB); now, we'll need one or more thread control blocks (TCBs) to store the state of each thread of a process. In the context switch we perform between threads, the address space remains the same (i.e., there is no need to switch which page table we are using).

In the address space, instead of a single stack for a single-threaded process, for a multi-threaded process, there will be one stack per thread in there. Sometimes called thread-local storage.

There are at least two major reasons you should use threads:
1. Parallelism. Divide the work among processors.
2. Avoid blocking program progress due to slow I/O. Using threads is a way to avoid getting stuck; while one thread in your program waits (i.e., is blocked waiting for I/O), the CPU scheduler can switch to other threads, which are ready to run and do something useful. Threading enables overlap of I/O with other activities within a single program, so many modern server-based applications (web servers, database management systems, ...) use threads in their implementations.

The objdump program is just one of many tools you should learn how to use; a debugger like gdb, memory profilers like valgrind or purify, and of course the compiler itself are others that you should spend time to learn more about; the better you are at using your tools, the better systems you'll be able to build.




