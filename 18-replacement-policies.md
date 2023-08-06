# 18. Beyond Physical Memory: Policies
How to decide which page to evict. Our goal in picking a replacement policy is to minimize the number of cache misses.

This decision is made by the replacement policy of the system, which usually follows some general principles, but also includes certain tweaks to avoid corner-case behaviors.

The optimal policy replaces the page, that will be accessed furthest in the future. It serves only as a comparison point.

The Least-Frequently-Used (LFU) policy replaces the least-frequently used page when an eviction must take place. Similarly, the Least-RecentlyUsed (LRU) policy replaces the least-recently-used page.

For most pages, the OS simply uses demand paging, which means the OS brings the page into memory when it is accessed, on demand. 

Prefetching: The OS could guess that a page is about to be used, and thus bring it in ahead of time.  Should only be done when there is reasonable chance of success.

Another policy determines how the OS writes pages out to disk. They could simply be written out one at a time; however, many systems, instead, collect a number of pending writes together in memory, and write them to disk in one (more efficient) write. This behavior is usually called clustering, or grouping of writes, and is effective because of the nature of disk drives, which perform a single large write more efficiently than many small ones.

What should the OS do, when memory is oversubscribed, and the memory demands of the running processes exceeds the available physical memory? In this case, the system will constantly be paging -> thrashing.

Some Linux versions run an out-of-memory killer, when memory is oversubscribed. This daemon chooses a memory intensive process and kills it. 
