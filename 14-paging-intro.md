# 14. Paging: Introduction

The OS takes one of two approaches when solving most space-management problems:
1. Chop things up into variable-sized pieces, like segmentation. However, when dividing a space into different-size chunks, the space itself can become fragmented, and thus allocation becomes more challenging over time.
2. Paging: Chop up virtual memory space into fixed-sized pieces (pages). Correspondingly, we view physical memory as an array of fixed-sized slots (page frames); each of these frames can contain a single virtual-memory page.

How to virtualize memory with pages?

Advantages: 
- Flexibility: the system can support the abstraction of an address space effectively, regardless of how a process uses the address space; we won't, for example, make assumptions about the direction the heap and stack grow and how they are used.
- Simplicity of free-space management. For example, when the OS wishes to place a 64-byte address space into a eight-page physical memory, it simply finds 4 free pages; perhaps the OS keeps a free list of all free pages, and just grabs the first four free pages off of this list.

Page table: A per-process data structure, stores virtual-to-physical address translations, letting the system know where each page of an address space actually resides in physical memory. One of the most important data structures in the memory management subsystem of a modern OS. 

Because each address space requires such translations, in general, there is one page table per process in the system.

Where Are Page Tables Stored? Because pages tables are big, so they are stored in memory somewhere.

The simplest data structure for a page table is a linear page table, which is an array. The OS indexes the array by the virtual page number (VPN), and looks up the page-table entry (PTE) at that index, in order to find the desired physical frame number (PFN).

What can be in a PTE:
- A "valid bit" is common, to indicate whether the particular translation is valid, such as whether an address is actually used by the process. 
- "Protection bits", indicating whether the page could be read from, written to, or executed from.
- A "present bit" indicates whether this page is in physical memory or on disk (i.e., it has been swapped out).
- A "dirty bit", indicating whether the page has been modified since it was brought into memory.
- A "reference bit" (accessed bit), used to track whether a page has been accessed. Useful to determine which pages are popular, and thus should be kept in memory. 

Implementing paging support without care will lead to a slower machine (with many extra memory accesses, to access the page table) as well as memory waste (with memory filled with page tables, instead of useful application data).

