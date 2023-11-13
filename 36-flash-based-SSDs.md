# 36. Flash-based SSDs
solid-state storage devices have no mechanical or moving parts like hard drives; rather, they are simply built out of transistors, much like memory and processors.

Flash has some unique properties. For example, to write to a given chunk of it (i.e., a flash page), you first have to erase a bigger chunk (i.e., a flash block), which can be quite expensive. In addition, writing too often to a page will cause it to wear out. These two properties make construction of a flash-based SSD an interesting challenge. 

Flash chips are designed to store one or more bits in a single transistor; the level of charge trapped within the transistor is mapped to a binary value. In a single-level cell (SLC) flash, only a single bit is stored within a transistor (i.e., 1 or 0); with a multi-level cell (MLC) flash, two bits are encoded into different levels of charge, e.g., 00, 01, 10, and 11 are represented by low, somewhat low, somewhat high, and high levels. There is even triple-level cell (TLC) flash, which encodes 3 bits per cell.

Flash chips are organized into banks or planes which consist of a large number of cells.

A bank is accessed in two different sized units: blocks, which are typically of size 128 KB or 256 KB, and pages, which are a few KB in size (e.g., 4KB). Within each bank there are a large number of blocks; within each block, there are a large number of pages.

Flash chips are pure silicon and in that sense have fewer reliability issues to worry about. The primary concern is wear out; when a flash block is erased and programmed, it slowly accrues a little bit of extra charge. Over time, as that extra charge builds up, it becomes increasingly difficult to differentiate between a 0 and a 1. At the point where it becomes impossible, the block becomes unusable.
