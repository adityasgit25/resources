So there are two kinds of threads:
1. Platform threads
2. Virtual threads

Plaform threads are nothing but just the wrapper over the OS threads, whereas Virtual threads are kinda tasks that managed by JVM.
Plaform threads are managed by the OS. So, also you can say the total number of OS threads = Platform threads.

1. Platform threads are the real workers, think of them like a worker.
2. Treat Virtual Threads as the tasks.
3. Its the Platform threads only that does the task.
4. There are virtual threads that mount on each Platform thread, so when any blocking calls happen, platform thread just unmount the virtual thread
and put the virtual thread there, and then frees itself(virtual thread) and go back to the Thread Pool looking for someother task, which does not happen
when there were not virtual thread, at that time, Platform thread has to wait there itself and hence the resources gets wasted.
5. You can create only a few thousand number of platform threads, but can create millions of virtual threads, platform threads are also heavy on the system.
6. When that blocking call unblocks then JVM again wakeup the virtual thread and also plaform thread comes and get the work done.

So this way its blocking but not wasting the resources and increases efficiency of the system.
That's what the goal of the reactive programming was, now that was done by the Virtual threads, still sometime you need reactive programming but major
this issue of blocking the resources have been highly eliminated.

And also, by this mechanism it also shows how the plaform threads can be reused.


Now you can write the blocking code and let the JVM runtime handle the scaling. Simple as that.
