# 23. Lock-based concurrent data structures
Adding locks to a data structure to make it usable by threads, makes the structure thread safe.

Ideally, you'd like to see the threads complete just as quickly on multiple processors, as the single thread does on one. Achieving this end is called perfect scaling; even though more work is done, it is done in parallel, and hence the time taken to complete the task is not increased.

- concurrent counters
- concurrent linked lists
- concurrent queues
- concurrent hash table
