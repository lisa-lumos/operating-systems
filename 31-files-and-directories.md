



# 31. Interlude: Files and Directories
How should the OS manage a persistent device? What are the APIs? What are the important aspects of the implementation?

Two key abstractions have developed over time in the virtualization of storage:
- File. A file is simply a linear array of bytes, each of which you can read or write. Each file has some kind of low-level name, usually a number of some kind; often, the user is not aware of this name. The low-level name of a file is often referred to as its inode number.
- Directory. A directory also has a low-level name, but it contains a list of (user-readable name, low-level name) pairs.

Each entry in a directory refers to either files or other directories. By nesting directories within other directories, users can build an arbitrary directory tree, under which all files and directories are stored.

Directories and files can have the same name, as long as they are in different locations in the file-system tree.

open() returns a file descriptor, which is managed by the operating system on a per-process basis.

Each running process already has 3 files open, standard input (which the process can read to receive input), standard output (which the process can write to in order to dump information to the screen), and standard error (which the process can write error messages to). These are represented by file descriptors 0, 1, and 2, respectively. Thus, when you first open another file (as cat does above), it will almost certainly be file descriptor 3.

Each process maintains an array of file descriptors, each of which refers to an entry in the system-wide "open file table". Each entry in this table tracks which underlying file the descriptor refers to, the current offset, and other relevant details, such as whether the file is readable or writable.

If a process that opens the same file twice, and issues a read to each of them, the offsets of the two file descriptors are tracked independently. Even if some other process reads the same file at the same time, each will have its own entry in the open file table. In this way, each logical reading or writing of a file is independent, and each has its own current offset while it accesses the given file.

However, there are a few interesting cases, where an entry in the open file table is shared. One of those cases occurs, when a parent process creates a child process with fork(). Sharing open file table entries across parent and child is occasionally useful. For example, if you create a number of processes that are cooperatively working on a task, they can write to the same output file without any extra coordination.









