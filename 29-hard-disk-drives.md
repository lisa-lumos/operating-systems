# 37. Hard Disk Drives
How do modern hard-disk drives store data? What is the interface? How is the data actually laid out and accessed? How does disk scheduling improve performance?

The basic interface for all modern drives is straightforward. The drive consists of n sectors/blocks (512-byte), each can be read or written. The sectors are numbered from 0 to n-1. Thus, we can view the disk as an array of sectors; 0 to n-1 is thus the address space of the drive.

Multi-sector operations are possible; file systems will read/write 4KB at a time, or more. However, when writing to the disk, only each sector write is atomic (it will either complete in its entirety, or it won't complete at all); thus, if an power loss occurs, only a portion of a larger write may complete (a torn write).

One can usually assume that, accessing two blocks near one-another within the drive's address space will be faster, than accessing two blocks that are far apart. One can also usually assume that, accessing blocks in a contiguous chunk (sequential r/w) is the fastest access mode, and usually much faster than any more random access pattern.

A platter is a circular hard surface, on which data is stored persistently by inducing magnetic changes to it. A disk may have one or more platters; each platter has 2 sides (surfaces). These platters are usually made of some hard material (such as aluminum), and then coated with a thin magnetic layer that enables the drive to persistently store bits even when the drive is powered off.

The platters are all bound together around the spindle, which is connected to a motor that spins the platters around at a fixed rate. The rate of rotation is often measured in rotations per minute (RPM), and typical modern values are in the 7,200 RPM to 15,000 RPM range.

Data is encoded on each surface in concentric circles of sectors; we call one such concentric circle a track. A single surface contains many tracks, tightly packed together, with hundreds of tracks fitting into the width of a human hair.

This process of reading/writing is accomplished by the disk head; there is one head per surface of the drive. The disk head is attached to a single disk arm, which moves across the surface to position the head over the desired track.

Seek: moving the disk arm to the correct track. Rotation: the rotation of splatter. Transfer: read the data. 

T_I/O = T_seek + T_rotation + T_transfer

Disk scheduling. Unlike job scheduling, where the length of each job is usually unknown, with disk scheduling, we can make a good guess at how long a job will take. By estimating the seek and possible rotational delay of a request, the disk scheduler can know how long each request will take, and thus (greedily) pick the one that will take the least time to service first(SJF, shortest job first) in its operation.
