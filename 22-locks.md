# 22. Locks
Programmers annotate source code with locks, putting them around critical sections, and thus ensure that any such critical section executes as if it were a single atomic instruction.

The semantics of the lock() and unlock() routines are simple. Calling the routine lock() tries to acquire the lock; if no other thread holds the lock, the thread will acquire the lock, and enter the critical section; this thread is sometimes said to be "the owner of the lock". If another thread then calls lock() on that same lock variable, it will not return while the lock is held by another thread; in this way, other threads are prevented from entering the critical section, while the first thread that holds the lock is in there.

Once the owner of the lock calls unlock(), the lock is now available/free again. If no other threads are waiting for the lock, the state of the lock is simply changed to "free". If there are waiting threads, one of them will (eventually) notice (or be informed of) this change of the lock's state, acquire the lock, and enter the critical section.

How to evaluate the efficacy of a particular lock implementation:
- achieves mutual exclusion
- fairness among threads to get the lock after the lock becomes free
- performance
