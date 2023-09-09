# 24. Condition Variables
Locks are not the only primitives that are needed for concurrent programs.

There are many cases where a thread wishes to check whether a condition is true, before continuing its execution. For example, a parent thread might wish to check whether a child thread has completed before continuing.

We need some way to put the parent to sleep, until the condition we are waiting for (e.g., the child is done executing) comes true.

To wait for a condition to become true, a thread can use a "condition variable", which is an explicit queue, that threads can put themselves on when some condition is not as desired (by waiting on the condition); some other thread, when it changes said state, can then wake one/more of those waiting threads, and thus allow them to continue (by signaling on the condition).
