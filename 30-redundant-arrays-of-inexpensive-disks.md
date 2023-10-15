# 30. Redundant arrays of inexpensive disks (RAIDs)
How can we make a large, fast, and reliable storage system? What are the key techniques? What are trade-offs between different approaches?

Redundant Array of Inexpensive Disks (RAID), a technique to use multiple disks in concert to build a faster, bigger, and more reliable disk system.

Externally, a RAID looks like a disk: a group of blocks one can read or write. Internally, the RAID is a complex beast, consisting of multiple disks, memory, and one or more processors to manage the system. A hardware RAID is very much like a computer system, specialized for the task of managing a group of disks.

RAIDs offer a number of advantages over a single disk. 
1. Performance. Using multiple disks in parallel can greatly speed up I/O times. 
2. Capacity. Large data sets demand large disks.
3. Reliability. Spreading data across multiple disks (without RAID techniques) makes the data vulnerable to the loss of a single disk; with some form of redundancy, RAIDs can tolerate the loss of a disk and keep operating as if nothing were wrong.

When considering how to add new functionality to a system, one should always consider whether such functionality can be added transparently, in a way that demands no changes to the rest of the system. RAIDs provide these advantages transparently to systems that use them, i.e., a RAID just looks like a big disk to the host system.
