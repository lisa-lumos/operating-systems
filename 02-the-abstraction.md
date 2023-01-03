# 2. The Abstraction: The Process
Virtualizing the CPU: Running one process, then stopping it and running another, and so forth, the OS can promote the illusion that many virtual CPUs exist when in fact there is only one or a few physical CPU. This is known as `time sharing of the CPU`, allows users to run as many concurrent processes as they would like; the potential cost is performance, as each will run more slowly if the CPU(s) must be shared.

The counterpart of time sharing is `space sharing`, where a resource is divided (in space) among those who wish to use it. For example, `disk space` is naturally a space-shared resource; once a block is assigned to a ﬁle, it is normally not assigned to another ﬁle until the user deletes the original ﬁle.

`Mechanisms` are low-level methods or protocols that implement a needed piece of functionality; e.g.: Context switch, which gives the OS the ability to stop running one program and start running another on the CPU. On top of these mechanisms are `policies` - algorithms for making some kind of decision within the OS; e.g.: A scheduling policy in the OS decides which program the OS run next.

A process API needs to include Create, Destroy, Wait, Miscellaneous Control, Status in the interface of an OS. 

Process creation: load program code and any static data (e.g., initialized variables) into memory (the address space of the process). Programs initially reside on disk in executable format; thus, the process of loading a program and static data into memory requires the OS to read those bytes from disk and place them in memory. Some memory must be allocated for the program’s run-time stack. The OS may also allocate some memory for the program’s heap. The OS will also do other initialization tasks related to input/output (I/O). Lastly, OS start the program at the entry point, the main(). By jumping to the main(), the OS transfers control of the CPU to the newly-created process, and the program begins its execution.
























