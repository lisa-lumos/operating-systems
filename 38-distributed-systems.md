# 38. Distributed Systems
The central tenet of modern networking is that communication is fundamentally unreliable.

Checksums are a commonly-used method to detect corruption quickly and effectively in modern systems.

How does the sender know that the receiver has actually received the message? The technique is known as an acknowledgment, or ack for short. The idea is: the sender sends a message to the receiver; the receiver then sends a short message back to acknowledge its receipt.

Timeout: When the sender sends a message, the sender now sets a timer to go off after some period of time. If, in that time, no acknowledgment has been received, the sender concludes that the message has been lost. The sender then simply performs a retry of the send, sending the same message again with hopes that this time, it will get through.

To enable the receiver to detect duplicate message transmission, the sender has to identify each message in some unique way, and the receiver needs some way to track whether it has already seen each message before. When the receiver sees a duplicate transmission, it simply acks the message, but does not pass the message to the application that receives the data. A simpler approach, requiring little memory, solves this problem, and the mechanism is known as a sequence counter.

Distributed shared memory (DSM) systems enable processes on different machines to share a large, virtual address space

Remote procedure call packages all have a simple goal: to make the process of executing code on a remote machine as simple and straightforward as calling a local function. 
