# The Memory API
The memory allocation interfaces in UNIX systems. 

In UNIX/C programs, understanding how to allocate/manage memory, is critical in building robust/reliable software.

In running a C program, there are two types of memory that are allocated:
1. Stack (automatic) memory. Allocations/de-allocations of it are managed implicitly by the compiler. For example, a variable defined in a function, which allocated automatically at the start, and de-allocated automatically after the function finishes - it doesn't live beyond the function's call invocation. 
2. Heap memory. Long-lived memory. All allocations/de-allocations are explicitly handled by the programmer. For example, spaces created using malloc(), and freed by free()





































