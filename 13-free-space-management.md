# 13. Free-space management
The problem of external fragmentation - the free space gets chopped into little pieces of different sizes (fragmented); subsequent requests may fail, because there is no single contiguous space that can satisfy the request, even though the total amount of free space exceeds the size of the request. 

Internal fragmentation: if an allocator hands out chunks of memory bigger than requested, these un-asked-for space, is internal fragmentation, because the waste occurs inside the allocated unit, and is another example of space waste.

Assumptions: 
- Assume no compaction of free space is possible. 
- Assume that the allocator manages a contiguous region of bytes

A free list: contains a set of elements, that describe the free space in the heap.

The `split` is commonly used in allocators, when requests are smaller than the size of any free chunk. Assume we have a request for 1 byte of memory. The allocator wil perform splitting: it will find a free chunk of memory that can satisfy the request, and split it into two. The 1st chunk it will return to the caller; the 2nd chunk will remain on the list.

`Coalescing` of free space: when an application calls free(...), if we just append a new node to the free list, indicting this free space, it require be more work to read, because it may connect with other free chunks, to form a larger continuous free chunk. Coalescing process solves this problem. 

Header: The interface to free(void * ptr) does not take a size parameter; thus it knows the size already. To make this work, most allocators store some info in a header block, which is kept in memory, right before the handed-out chunk of memory.

The ideal allocator is both fast and minimizes fragmentation. Here are some basic strategies for managing free space:
- Best fit: search through the free list to find large-enough ones, return the smallest one. Disadvantage: exhaustive search takes time
- Worst fit: find the largest chunk and return the requested amount. 
- First fit: finds the first block that is big enough, and returns the requested amount
- Next fit: keeps an extra pointer to the location within the list, where one was looking previously. The idea is to spread the searches for free space throughout the list more uniformly, thus avoiding splintering of the beginning of the list.

Other approaches 、： 















