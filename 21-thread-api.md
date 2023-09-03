# 21. Thread API
- Thread creation `pthread_create(...)`
- Thread completion `pthread_join(...)`

Tips for writing multi-threaded code: 
- Keep it simple. Tricky thread interactions lead to bugs.
- Minimize thread interactions. 
- Initialize locks and condition variables. Failure to do so will lead to code that sometimes works and sometimes fails, in very strange ways.
- Check your return codes. Failure to do so will lead to bizarre and hard to understand behavior.
- Be careful with how you pass arguments to, and return values from, threads. In particular, any time you are passing a reference to a variable allocated on the stack, you are probably doing something wrong.
- Each thread has its own stack. To share data between threads, the values must be in the heap or otherwise some locale that is globally accessible.
- Always use condition variables to signal between threads. While it is often tempting to use a simple flag, don't do it.
- Read the docs.
