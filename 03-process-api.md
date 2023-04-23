# 3. Process API
Process creation in UNIX systems. 

## The fork() System Call
Used to create a new process. in UNIX systems, the PID (process identifier) is used to name the process if one wants to do something with it, such as, stop this process. When the parent receives the PID of the newly-created child, the child receives a return code of zero.

The creator is called the parent; the newly created process is called the child. The child process is a nearly identical copy of the parent.

## The wait() System Call
For a parent to wait for a child process to finish, then resume execution.

## The exec() System Call
To run a program that is different from the calling program. The exec() family of system calls allows a child to break free from its similarity to its parent and execute an entirely new program.

The separation of fork() and exec() is essential in building a UNIX shell, because it lets the shell run code after fork() but before exec(); this code can alter the environment of the about-to-be-run program, and thus enables a variety of interesting features to be readily built. The separation of fork and exec enables features like input/output redirection, pipes, and other cool features, all without changing anything about the programs being run.

The shell is just a user program. It shows you a prompt and then waits for you to type something into it. You then type a command (executable program name, plus any arguments) into it; in most cases, the shell then figures out where in the file system the executable resides, calls fork() to create a new child process to run the command, calls some variant of exec() to run the command, and then waits for the command to complete by calling wait(). When the child completes, the shell returns from wait() and prints out a prompt again, ready for your next command.

You can also redirect the output of a program into a file. UNIX pipes are implemented in a similar way, but with the pipe() system call.

## Process Control and Users
Process control is available in the form of signals, which can cause jobs to stop, continue, or even terminate. Beyond fork(), exec(), and wait(), there are many other interfaces for interacting with processes in UNIX systems.

The entire signals subsystem provides a rich infrastructure to deliver external events to processes, including ways to receive and process those signals within individual processes, and ways to send signals to individual processes as well as entire process groups.

Users generally can only control their own processes; it is the job of the OS to parcel out resources (such as CPU, memory, and disk) to each user (and their processes) to meet overall system goals.

A system generally needs a user who can administer the system, they can kill an arbitrary process even though that process was not started by this user, shudown the system, etc. In UNIX-based systems, these abilities are given to the superuser (root).













