# 15. Paging: Faster Translations
Address-translation cache:
To speed address translation, we need a translation-lookaside buffer (TLB), which is part of the chip's memory-management unit (MMU), and is a hardware cache of popular virtual-to-physical address translations; thus, a better name would be an "address-translation cache".

Upon each virtual memory reference, the hardware first checks the TLB, to see if the desired translation already there; if so, the translation is performed quickly, without having to consult the page table. Because of their tremendous performance impact, TLBs make virtual memory possible. 

Caching is one of the most fundamental performance techniques in computer systems. The idea behind hardware caches, is to take advantage of locality in instruction and data references. There are usually two types of locality: temporal locality and spatial locality.Temporal locality: an instruction or data item that has been recently accessed will likely be re-accessed soon in the future. Spatial locality: if a program accesses memory at address x, it will likely soon access memory near x.

Which TLB entry should be replaced, when we add a new TLB entry? One common approach is to evict the least-recently-used (LRU) entry. Another typical approach is to use a random policy, which evicts a TLB mapping at random, which avoids corner-case behaviors.

