# 13. Free-space management
The problem of external fragmentation - the free space gets chopped into little pieces of different sizes (fragmented); subsequent requests may fail, because there is no single contiguous space that can satisfy the request, even though the total amount of free space exceeds the size of the request. 

Internal fragmentation: if an allocator hands out chunks of memory bigger than requested, these un-asked-for space, is internal fragmentation, because the waste occurs inside the allocated unit, and is another example of space waste.

Assumptions: 
- Assume no compaction of free space is possible. 
- Assume that the allocator manages a contiguous region of bytes





























