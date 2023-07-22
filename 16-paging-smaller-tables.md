# 16. Paging: Smaller Tables
Simple array-based page tables (linear page tables) are too big, taking up far too much memory on typical systems. How can we make page tables smaller?

## Bigger pages
We could reduce the size of the page table if we use bigger pages.

The major problem with this approach, is that big pages lead to waste within each page (internal fragmentation). 

## Hybrid Approach: Paging and Segments
Combining paging and segmentation, to reduce the memory overhead of page tables.

## Multi-level Page Tables
It turns the linear page table into something like a tree.

First, chop up the page table into page-sized units; then, if an entire page of page table entries is invalid, don't allocate that page of the page table at all. To track whether a page of the page table is valid (and if valid, where it is in memory), use the page directory. The page directory thus either can be used to tell you where a page of the page table is, or that the entire page of the page table contains no valid pages.
