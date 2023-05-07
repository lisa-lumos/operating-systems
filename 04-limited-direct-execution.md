# 4. Limited direct execution
Obtaining high performance while maintaining control is one of the central challenges in building an OS.

Direct execution: when the OS want to start to run a program, it creates a process entry for it in a process list, allocates some memory for it, loads the program code into memory from disk, locates its entry point, eg, the main() routine, jumps to it, and starts running the user's code.

A new processor mode, the `user mode` - code that runs in user mode is restricted in what it can do. Eg, when running in user mode, a process can't issue I/O requests; doing so would result in the processor raising an exception; the OS would then likely kill the process.

In contrast to user mode is `kernel mode`, which the OS (or kernel) runs in. In this mode, code that runs can do what it likes, including privileged operations such as issuing I/O requests and executing all types of restricted instructions.

`System calls` allow the kernel to expose some key functionality to user programs, such as accessing the file system, creating and destroying processes, communicating with other processes, and allocating more memory. To execute a system call, a program must execute a special trap instruction. This instruction simultaneously jumps into the kernel and raises the privilege level to kernel mode. When finished, the OS calls a return-from-trap instruction, which returns into the calling user program while simultaneously reducing the privilege level back to user mode. On x86, the processor will push the program counter/flags/registers onto a per-process kernel stack; the return-from trap will pop these values off the stack and resume execution of the user mode program. 

There are two phases in the limited direct execution (LDE) protocol. In the first phase (at boot time), the kernel initializes the trap table, and the CPU remembers its location for subsequent use. The kernel does so via a privileged instruction. In the second phase (when running a process), the kernel sets up a few things (e.g., allocating a node on the process list, allocating memory) before using a return-from-trap instruction to start the execution of the process; this switches the CPU to user mode and begins running the process. When the process wishes to issue a system call, it traps back into the OS, which handles it and once again returns control via a return-from-trap to the process. The process then completes its work, and returns from main(); this usually will return into some stub code which will properly exit the program (say, by calling the exit() system call, which traps into the OS). At this point, the OS cleans up and we are done.

In a cooperative scheduling system, the OS regains control of the CPU by waiting for a system call or an illegal operation of some kind to take place.

Non-Cooperative: A timer device can be programmed to raise an `timer interrupt` every so many milliseconds; when the interrupt is raised, the currently running process is halted, and a pre-configured interrupt handler in the OS runs - the OS has regained control of the CPU, and thus can do what it wants: stop the current process, and start a different one.

`Context switch`: the OS saves a few register values for the currently-executing process (eg, onto its kernel stack) and restore a few for the soon-to-be-executing process (from its kernel stack). 





















