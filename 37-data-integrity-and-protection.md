# 37. Data Integrity and Protection
how should a file system or storage system ensure that data is safe, given the unreliable nature of modern storage devices?

2 types of single-block failures are common: latent-sector errors (LSEs) and block corruption.

LSEs arise when a disk sector (or group of sectors) has been damaged in some way. Solution: redundancy. 

The primary mechanism used by modern storage systems to preserve data integrity is called the checksum.

A checksum is the result of a function, that takes a chunk of data as input, and computes a function over said data, producing a small summary of the contents of the data. This summary is referred to as the checksum. 
