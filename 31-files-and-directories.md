



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

Most times when a program calls write(), it is just telling the file system: please write this data to persistent storage, at some point in the future (eventual guarantee). However, some applications require more than this, e.g., in a database management system (DBMS), development of a correct recovery protocol requires the ability to force writes to disk from time to time. To support these types of applications, most file systems provide some additional control APIs, such as fsync().

The rename() call is usually implemented as an atomic call, with respect to system crashes; if the system crashes during the renaming, the file will either be named the old name or the new name, and not an odd in-between state. 

Beyond file access, we expect the file system to have information about each file it is storing (files metadata). 

Beyond files, a set of directory-related system calls enable you to make, read, and delete directories.

Hard links are somewhat limited: you can't create one to a directory (for fear that you will create a cycle in the directory tree); you can't hard link to files in other disk partitions (because inode numbers are only unique within a particular file system, not across file systems). Symbolic link (soft link) solves this problem. 

symbolic link is actually a file itself, of a different type. Because of the way symbolic links are created, they leave the possibility for dangling reference.

Similar to CPU and memory virtualizations for a process, the file system also presents a virtual view of a disk, transforming it from a bunch of raw blocks into much more user-friendly files and directories. 

However, the abstraction is different from that of the CPU and memory - files are commonly shared among different users and processes and are not (always) private.

Thus, a more comprehensive set of mechanisms for enabling various degrees of sharing, are usually present within file systems.
- The first form of such mechanisms is the classic U NIX permission bits. The permissions consist of 3 groupings: what the owner of the file can do to it, what someone in a group can do to the file, and, what anyone (other) can do.

On local file systems, the common default is for there to be some kind of superuser (i.e., root), who can access all files, regardless of privileges. In a distributed file system, such as AFS (which has access control lists), a group called "system:administrators" contains users that are trusted to do so.

Beyond permissions bits, some file systems, such as the distributed file system known as AFS, include more sophisticated controls. AFS, for example, does this in the form of an access control list (ACL) per directory. Access control lists are a more general and powerful way, to represent exactly who can access a given resource. In a file system, this enables a user to create a very specific list, of who can and cannot read a set of files, in contrast to the somewhat limited owner/group/everyone model of permissions bits described above.

Instead of having a number of separate file systems, mount unifies all file systems into one tree, making naming uniform and convenient.
