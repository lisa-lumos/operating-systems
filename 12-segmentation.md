# Segmentation
Problem of current model: there is a big chunk of "free" space right in the middle, between the stack and the heap. It also makes it hard to run a program, when the entire address space doesn't fit into memory - it is not flexible. 

Solution: segmentation. Instead of having just one base and bounds pair, have a base and bounds pair per logical segment. This allows the OS to place each segments (code/heap/stack) independently in different parts of physical memory, and thus avoid filling physical memory with unused virtual address space.

Segmentation fault: on a segmented machine, an attempt to access an illegal address.

The explicit approach to know which segment(code/heap/stack) an address refers - Use the first 2 bits to map to one of the three type of segments. And the bottom 12 bits are th offset into the segment. 

The implicit approach: If the address was generated from the program counter (an instruction fetch), then the address is within the code segment; if the address is based off of the stack or base pointer, it must be in the stack segment; any other address must be in the heap.

In addition to base and bounds values, the hardware also needs to know which way the segment grows (a bit, for example, that is set to 1 when the segment grows in the positive direction, and 0 for negative).

To save memory, it is useful to share certain memory segments between address spaces. Code sharing is common and still in use in systems today. Basic support adds a few bits per segment, indicating whether or not a program can read or write a segment, or perhaps execute code that lies within the segment.

With protection bits, the hardware algorithm described earlier would also have to change. In addition to checking whether a virtual address is within bounds, the hardware also has to check whether a particular access is permissible.

Each process has its own virtual address space, so on a context switch, the OS must make sure to set up these registers correctly before letting the process run again.

