# 14. Paging: Introduction

The OS takes one of two approaches when solving most space-management problems:
1. Chop things up into variable-sized pieces, like segmentation. However, when dividing a space into different-size chunks, the space itself can become fragmented, and thus allocation becomes more challenging over time.
2. Paging: Chop up virtual memory space into fixed-sized pieces (pages). Correspondingly, we view physical memory as an array of fixed-sized slots (page frames); each of these frames can contain a single virtual-memory page.

How to virtualize memory with pages?

Advantages: 
- Flexibility: the system can support the abstraction of an address space effectively, regardless of how a process uses the address space; we won't, for example, make assumptions about the direction the heap and stack grow and how they are used.
- Simplicity of free-space management. For example, when the OS wishes to place a 64-byte address space into a eight-page physical memory, it simply finds 4 free pages; perhaps the OS keeps a free list of all free pages, and just grabs the first four free pages off of this list.

Page table: A per-process data structure, store address translations for each virtual pages of the address space, for the OS to map them to physical memory. 

Where Are Page Tables Stored?





















