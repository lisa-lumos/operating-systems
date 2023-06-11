# 11. Mechanism: Address Translation
How to virtualize memory efficiently and flexibly. 

Hardware-based address translation (aka, address translation): For each memory reference, an address translation is performed by the hardware, to redirect application memory references to their actual locations in memory. The OS must get involved to set up the hardware, so that the correct translations take place - it must manage memory, keep track of which locations are free/occupied, and maintain control over how memory is used.

Dynamic (Hardware-based) Relocation (aka, base and bounds): have two hardware registers within each CPU: the base register, and the bounds register. `physical address = virtual address + base`. The processor will check that the memory reference is within bounds to make sure it is legal, otherwise the CPU will raise an exception, and the process will likely be terminated. 

The hardware should provide special instructions to modify the base and bounds registers, allowing the OS to change them when different processes run. These instructions are privileged; only in kernel (or privileged) mode can the registers be modified.

Critical junctures where the OS must get involved, to implement base-and-bounds version of virtual memory:
1. When a process is created, the OS must find space for its address space in memory. The OS will search a data structure (a free list) to find room for the new address space and then mark it used.
2. When a process is terminated, the OS must reclaim all of its memory, for use in other processes or the OS. It will putt its memory back on the free list, and cleans up any associated data structures.
3. When a context switch occurs. There is only pair of base and bounds register pair on each CPU, and their values differ for each running program, because each program is loaded at a different physical address in memory. Thus, the OS must save and restore the base-and-bounds pair, when it switches between processes.
4. the OS must provide exception handler (functions); the OS installs these handlers at boot time, via privileged instructions. For example, if a process tries to access memory outside its bounds, the CPU will raise an exception. 

when a process is not running, it is possible for the OS to move an address space from one location in memory to another. To do so, the OS first deschedules the process; then, the OS copies the address space from the current location to the new location; finally, the OS updates the saved base register (in the process structure) to point to the new location. 

