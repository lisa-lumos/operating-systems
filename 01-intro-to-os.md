# 1. Introduction to Operating Systems
`Virtualization`: The OS takes a physical resource (processor/memory/a disk) and transforms it into a more general virtual form of itself. The OS also provides some interfaces (APIs/system calls/standard library, all mean the same) so you can interact with it.

Because virtualization allows many programs to run (sharing the CPU), and many programs to concurrently access their own instructions and data (sharing memory), and many programs to access devices (sharing disks etc.), the OS is sometimes known as a `resource manager`. Each of the CPU, memory, and disk is a resource of the system; it is the operating system’s role to manage those resources. 

## Virtualizing The CPU
Turning a single CPU (or a small set of them) into a seemingly infinite number of CPUs and thus allowing many programs to seemingly run at once is what we call `virtualizing the CPU`. 

`Policies` are used in many different places within an OS to decide which program to run when they all request to run at the same time, etc. Hence the role of the OS as a `resource manager`.

## Virtualizing Memory
Memory is accessed all the time when a program is running. A program keeps all of its data structures in memory, and accesses them through various instructions. Each instruction of the program is in memory too; thus memory is accessed on each instruction fetch.

When the OS is virtualizing memory, each process accesses its own private virtual address space, which the OS somehow maps onto the physical memory of the machine. A memory reference within one running program does not affect the address space of other processes (or the OS itself); as far as the running program is concerned, it has physical memory all to itself. The reality, however, is that `physical memory is a shared resource, managed by the operating system`.

For modern multi-threaded programs, you can think of a thread as a function running within the same memory space as other functions, with more than one of them active at a time. When two threads increment a shared counter, a counter increment takes three instructions: one to load the value of the counter from memory into a register, one to increment it, and one to store it back into memory. Because these three instructions do not execute atomically (all at once), strange things can happen. 

## Persistence

































