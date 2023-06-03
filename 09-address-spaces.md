# 9. The Abstraction: Address Spaces
From the memory perspective, early machines didn't provide abstraction to users. The OS was a library that sat in memory,starting at physical address 0, and there would be one running program (a process) that currently sat in physical memory (eg, starting at physical address 64k) and used the rest of memory.

Because machines were expensive, people began to share machines more effectively - multiprogramming. Interactivity became important, as users might be concurrently using a machine, each waiting for a timely response from their currently-executing tasks. 

While saving and restoring register-level state is fast, saving the entire contents of memory to disk is non-performant. Thus, we'd rather leave processes in memory while switching between them, allowing the OS to implement time sharing efficiently. But this makes protection an issue - you don't want a process to be able to read/write some other process's memory.


































