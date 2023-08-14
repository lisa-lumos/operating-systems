# 19. Complete Virtual Memory Systems
Linux development has been driven forward by real engineers solving real problems encountered in production, and thus a large number of features have slowly been incorporated into what is now a fully functional, feature-filled virtual memory system.

Like other modern OS, a Linux virtual address space consists of a user portion (where user program code, stack, heap, and other parts reside) and a kernel portion (where kernel code, stacks, heap, and other parts reside). Upon a context switch, the user portion of the currently-running address space changes; the kernel portion is the same across processes. Like those other systems, a program running in user mode cannot access kernel virtual pages; only by trapping into the kernel and transitioning to privileged mode can such memory be accessed.

One slightly interesting aspect of Linux is that it contains two types of kernel virtual addresses. 
- The first are known as kernel logical addresses. This is what you would consider the normal virtual address space of the kernel; to get more memory of this type, kernel code merely needs to call kmalloc. Suitable for operations which need contiguous physical memory to work correctly, such as I/O transfers to and from devices via directory memory access (DMA). 
- The other type of kernel address is a kernel virtual address. To get memory of this type, kernel code calls a different allocator, vmalloc, which returns a pointer to a virtually contiguous region of the desired size.

Intel x86 allows for the use of multiple page sizes, not just the standard 4KB page. Specifically, recent designs support 2-MB and even 1-GB pages in hardware. Thus, over time, Linux has evolved to allow applications to utilize these huge pages (as they are called in the world of Linux).

Linux developers have added transparent huge page support. When this feature is enabled, the operating system automatically looks for opportunities to allocate huge pages (usually 2 MB, but on some systems, 1 GB) without requiring application modification. Huge pages are not without their costs. The biggest potential cost is internal fragmentation. 

The Linux page cache is unified, keeping pages in memory from three primary sources: memory-mapped files, file data and metadata from devices, and heap/stack pages that comprise each process. These entities are kept in a page cache hash table, allowing for quick lookup when data is needed.

Buffer overflow attacks can be used against normal user programs, and even the kernel itself. The idea of these attacks is to find a bug in the target system, which lets the attacker inject arbitrary data into the target's address space. Such vulnerabilities sometime arise because the developer assumes (erroneously) that an input will not be overly long, and thus (trustingly) copies the input into a buffer; because the input is in fact too long, it overflows the buffer, thus overwriting memory of the target.

If successful upon a network-connected user program, attackers can run arbitrary computations or even rent out cycles on the compromised system; if successful upon the operating system itself, the attack can access even more resources, and is a form of what is called privilege escalation. 

Return-oriented programming (ROP): arbitrary code sequences can be executed by malicious code. Address space layout randomization (ASLR): Instead of placing code/stack/heap at fixed locations within the virtual address space, the OS randomizes their placement, thus making it quite challenging to craft the intricate code sequence required to implement this class of attacks. ASLR is such a useful defense for user-level programs that it has also been incorporated into the kernel, in a feature unimaginatively called kernel address space layout randomization (KASLR).
