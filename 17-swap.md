# 17. Swap
To support large address spaces, the OS will need a place to stash away address spaces that currently aren't in great demand. In modern systems, this role is usually served by a hard disk drive.

A contrast is found in older systems that used memory overlays, which required programmers to manually move pieces of code or data in/out of memory as they were needed. 

Beyond just a single process, the addition of swap space allows the OS to support the illusion of a large virtual memory for multiple concurrently-running processes.

The first thing we will need to do, is to reserve some space (swap space) on the disk, for moving pages back and forth. 

When the hardware looks in the PTE, it may find that the page is not present in physical memory. The way the hardware (or the OS) determines this, is through a new piece of information in each page-table entry, known as the present bit. If it is set to zero, the page is not in memory, but rather on disk somewhere.

The act of accessing a page, that is not in physical memory, is referred to as a page fault. The OS is invoked to service the page fault, with page-fault handler. The OS will need to swap the page into memory, in order to service the page fault. It looks in the PTE to find the address, and issues a request to disk, to fetch the page into memory.

If the memory is close to full, the OS may first page out one/more pages, to make room for the new page(s). The process of picking a page to kick out, or replace, is known as the page-replacement policy.












