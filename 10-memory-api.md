# The Memory API
The memory allocation interfaces in UNIX systems. 

In UNIX/C programs, understanding how to allocate/manage memory, is critical in building robust/reliable software.

In running a C program, there are two types of memory that are allocated:
1. Stack (automatic) memory. Allocations/de-allocations of it are managed implicitly by the compiler. For example, a variable defined in a function, which allocated automatically at the start, and de-allocated automatically after the function finishes - it doesn't live beyond the function's call invocation. 
2. Heap memory. Long-lived memory. All allocations/de-allocations are explicitly handled by the programmer. For example, spaces created using malloc(), and freed by free(). 

Many newer languages support automatic memory management - you call "new" to allocate a new object, but you never have to manually free the space; rather, a garbage collector figures out what memory you no longer have references to, and frees it for you.

Buffer overflow: not allocating enough memory.

Memory leak: occurs when you forget to free memory. Note that using a garbage-collected language doesn't help - if you still have a reference to some chunk of memory, no garbage collector will ever free it, and thus memory leaks remain a problem even in more modern languages.

malloc() and free() are not system calls, but library calls. Thus the malloc library manages space within your virtual address space, but itself is built on system calls, which call into the OS to ask for more memory, or release some back to the system.

