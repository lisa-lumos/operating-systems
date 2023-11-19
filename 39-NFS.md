# 39. Sun's Network File System (NFS)
How do you build a distributed file system?

One of the earliest and quite successful distributed systems was developed by Sun Microsystems, and is known as the Sun Network File System (or NFS). 

Sun instead developed an open protocol which simply specified the exact message formats that clients and servers would use to communicate. Different groups could develop their own NFS servers and thus compete in an NFS marketplace while preserving interoperability.

The ability of the client to simply retry the request (regardless of what caused the failure) is due to an important property of most NFS requests: they are idempotent.

Client-side caching: The NFS client-side file system caches file data (and metadata) that it has read from the server in client memory. Thus, while the first access is expensive (i.e., it requires network communication), subsequent accesses are serviced quite quickly out of client memory.
