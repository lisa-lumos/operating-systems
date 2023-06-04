# 9. The Abstraction: Address Spaces
From the memory perspective, early machines didn't provide abstraction to users. The OS was a library that sat in memory,starting at physical address 0, and there would be one running program (a process) that currently sat in physical memory (eg, starting at physical address 64k) and used the rest of memory.

Because machines were expensive, people began to share machines more effectively - multiprogramming. Interactivity became important, as users might be concurrently using a machine, each waiting for a timely response from their currently-executing tasks. 

While saving and restoring register-level state is fast, saving the entire contents of memory to disk is non-performant. Thus, we'd rather leave processes in memory while switching between them, allowing the OS to implement time sharing efficiently. But this makes protection an issue - you don't want a process to be able to read/write some other process's memory.

## The address space
The address space: an easy to use abstraction of physical memory. It is the running program's view of memory in the system.

The address space of a process contains all of the memory state of the running program. 

For example, 
- The "code" (instructions) of the program live somewhere in memory, and thus they are in the address space. 
- The running program uses a "stack" to keep track of where it is in the function call chain, as well as to allocate local variables, and pass parameters and return values to and from routines. 
- The "heap" is used for dynamically-allocated, user-managed memory, like, from malloc() in C, or "new" in C++ or Java.

Memory virtualization: The OS builds abstraction of a private, potentially large address space for multiple running processes (all sharing memory) on top of a single, physical memory. 

Goals of the OS:
- Transparency: The program is not aware of the fact that memory is virtualized; rather, the program behaves as if it has its own private physical memory.
- Efficiency: time and space
- Protection: processes/OS should not affect each other's memory. 

If you print out an address in a program, it's a virtual one, an illusion of how things are laid out in memory; only the OS (and the hardware) knows the real truth. For any C program that prints out a pointer - The value you see (some large number, often printed in hexadecimal), is a virtual address. Any address you can see as a programmer of a user-level program is a virtual address.

