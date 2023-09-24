# 28. I/O Devices
How should I/O be integrated into systems? What are the general mechanisms? How can we make them efficient?

The picture below is a classical diagram of a typical system. It shows a single CPU attached to the main memory of the system, via some kind of memory bus or interconnect. Some devices are connected to the system via a general I/O bus, which in many modern systems would be PCI; graphics and some other higher-performance I/O devices might be found here. Finally, even lower down are one or more peripheral bus, such as SCSI, SATA, or USB. These connect slow devices to the system, including disks, mice, and keyboards.

<img src="images/a-typical-system.png" style="width:50%;"/>

Why do we need a hierarchical structure like this? Physics, and cost. The faster a bus is, the shorter it must be; thus, a high-performance memory bus does not have much room to plug devices into it. In addition, engineering a bus for high performance is costly. Thus, system designers have adopted this hierarchical approach, where components that demand high performance (such as the graphics card) are nearer the CPU. Lower performance components are further away. The benefits of placing disks and other slow devices on a peripheral bus are manifold; in particular, you can place a large number of devices on it.

Modern systems increasingly use specialized chipsets, and faster point-to-point interconnects to improve performance.

A canonical device has two important components:
1. Interface. The hardware interface it presents to the rest of the system, such as status/command/data registers.
2. Internals. Its internal structure, such as CPU, memory, etc

The simplified device interface is comprised of three registers: a status register, which can be read to see the current status of the device; a command register, to tell the device to perform a certain task; and a data register to pass/get data to/from the device. By reading/writing these registers, the operating system can control device behavior.

A typical interaction that the OS might have with the device:
1. The OS waits until the device is ready to receive a command, by repeatedly reading the status register (polling the device)
2. the OS sends data to the data register. When the main CPU is involved with the data movement, we refer to it as programmed I/O (PIO). 
3. the OS writes a command to the command register, lets the device know that the data is present, and that it should begin working on the command. 
4. the OS waits for the device to finish, by polling it in a loop, waiting to see if it is finished (it may then get an error code to indicate success/failure).

How can the OS check device status without frequent polling, to lower the CPU overhead?

The interrupt. Instead of polling the device repeatedly, the OS can issue a request, put the calling process to sleep, and context switch to another task. When the device is finished with the operation, it will raise a hardware interrupt, causing the CPU to jump into the OS, at a predetermined interrupt service routine (ISR) or an interrupt handler.

The handler is a piece of operating system code that will finish the request, and wake the process waiting for the I/O.

If the speed of the device is not known, or sometimes fast and sometimes slow, it may be best to use a hybrid that polls for a little while and then, if the device is not yet finished, uses interrupts. This two-phased approach may achieve the best of both worlds.

Another interrupt-based optimization is coalescing. A device which needs to raise an interrupt first waits for a while, before delivering the interrupt to the CPU. While waiting, other requests may soon complete, and thus multiple interrupts can be coalesced into a single interrupt delivery, lowering the overhead of interrupt processing. Of course, waiting too long will increase the latency of a request, a common trade-off in systems.

When using programmed I/O (PIO) to transfer a large chunk of data to a device, the CPU can be overburdened with a rather trivial task, and thus wastes a lot of time/effort that could better be spent running other processes.

With PIO, the CPU spends too much time moving data to and from devices by hand. How can we offload this work and thus allow the CPU to be more effectively utilized?

Direct Memory Access (DMA). A DMA engine is essentially a specific device, that can orchestrate transfers between devices and main memory, without much CPU intervention.

DMA works as follows: To transfer data to the device, the OS would program the DMA engine, by telling it where the data lives in memory, how much data to copy, and which device to send it to. At that point, the OS is done with the transfer and can proceed with other work. When the DMA is complete, the DMA controller raises an interrupt, and the OS thus knows the transfer is complete.

How should the hardware communicate with a device? Should there be explicit instructions? Or are there other ways to do it?

The first, oldest method is to have explicit I/O instructions. Such instructions are usually privileged. The OS controls devices, and the OS thus is the only entity allowed to directly communicate with them.

The second method to interact with devices is memory mapped I/O. The hardware makes device registers available, as if they were memory locations. To access a particular register, the OS issues a load (to read) or store (to write) the address; the hardware then routes the load/store to the device instead of main memory.

Both approaches are still in use today.

How can we keep most of the OS device-neutral, thus hiding the details of device interactions from major OS subsystems?

The problem is solved through abstraction. At the lowest level, a piece of software (device driver) in the OS must know in detail how a device works.

For example, a file system is completely oblivious to the specifics of which disk class it is using; it simply issues block read/write requests to the generic block layer, which routes them to the appropriate device driver, which handles the details of issuing the specific request.









