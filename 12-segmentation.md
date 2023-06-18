# Segmentation
Problem of current model: there is a big chunk of "free" space right in the middle, between the stack and the heap. It also makes it hard to run a program, when the entire address space doesn't fit into memory - it is not flexible. 

Solution: segmentation. Instead of having just one base and bounds pair, have a base and bounds pair per logical segment. This allows the OS to place each segments (code/heap/stack) independently in different parts of physical memory, and thus avoid filling physical memory with unused virtual address space.

Segmentation fault: on a segmented machine, an attempt to access an illegal address.

An explicit approach to know which segment(code/heap/stack) an address refers - Use the first 2 bits to map to one of the three type of segments. And the bottom 12 bits are th offset into the segment. 































