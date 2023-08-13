# 19. Complete Virtual Memory Systems
Linux development has been driven forward by real engineers solving real problems encountered in production, and thus a large number of features have slowly been incorporated into what is now a fully functional, feature-filled virtual memory system.

Like other modern OS, a Linux virtual address space consists of a user portion (where user program code, stack, heap, and other parts reside) and a kernel portion (where kernel code, stacks, heap, and other parts reside). Upon a context switch, the user portion of the currently-running address space changes; the kernel portion is the same across processes. Like those other systems, a program running in user mode cannot access kernel virtual pages; only by trapping into the kernel and transitioning to privileged mode can such memory be accessed.

One slightly interesting aspect of Linux is that it contains two types of kernel virtual addresses. 
- The first are known as kernel logical addresses. This is what you would consider the normal virtual address space of the kernel; to get more memory of this type, kernel code merely needs to call kmalloc. Suitable for operations which need contiguous physical memory to work correctly, such as I/O transfers to and from devices via directory memory access (DMA). 
- The other type of kernel address is a kernel virtual address. To get memory of this type, kernel code calls a different allocator, vmalloc, which returns a pointer to a virtually contiguous region of the desired size.































