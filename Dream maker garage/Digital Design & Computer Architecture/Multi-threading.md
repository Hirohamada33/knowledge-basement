Multithreading is a programming and execution model that allows multiple threads of execution to run concurrently within the same process. Multithreading is a powerful technique for building concurrent and responsive applications, it can significantly enhance the performance and scalability of software systems. 

Some key points for a multi-threading method:
1. **Concurrency**: Multithreading enables concurrency by allowing multiple threads to execute independently and concurrently within the same program. Each thread runs its own sequence of instructions, allowing different tasks to progress simultaneously.
2. **Shared Resources**: Threads within the same process share the same memory space and resources. This means they can access and modify shared data, variables, and resources concurrently. However, concurrent access to shared resources can lead to synchronization issues and data inconsistencies if not properly managed.
3. **Responsiveness**: Multithreading can improve the responsiveness of applications, especially in user interfaces and server applications. By running tasks concurrently, the application can remain responsive to user input or handle multiple client requests simultaneously without blocking.
4. **Parallelism**: Multithreading can also enable parallelism, where multiple threads execute different parts of a task simultaneously on multi-core processors. This can lead to performance improvements by leveraging the computational power of multiple CPU cores.
5. **Thread Management**: Multithreading requires careful management of threads, including creating, starting, pausing, resuming, and terminating threads. Thread management also involves synchronization mechanisms to coordinate access to shared resources and prevent race conditions and data corruption.
6. **Thread Safety**: Writing multithreaded code requires attention to thread safety to ensure that shared data structures and resources are accessed in a safe and predictable manner. Techniques such as locks, mutexes, semaphores, and atomic operations are used to protect critical sections of code and enforce mutual exclusion.

A **thread** is a path which is followed during a program’s execution. Majority of programs written now a days run as a single thread. A process is a program being executed. A process can be further divided into independent units known as threads, hence a thread is like a small light-weight process within a process. 
Threading is a segment which divide the code into small parts that are of very light weight and has less burden on CPU memory so that it can be easily worked out and can achieve goal in desired field. The concept of threading is designed due to the problem of fast and regular changes in technology and less the work in different areas due to less application.

Some common application of multi-threading: transaction processing, recharges, online transfer, banking. 

**Life cycles of a thread**:
1. **New:** The lifecycle of a born thread (new thread) starts in this state. It remains in this state till a program starts.
2. **Runnable**: A thread becomes runnable after it starts. It is considered to be executing the task given to it.
3. **Waiting**: While waiting for another thread to perform a task, the currently running thread goes into the waiting state and then transitions back again after receiving a signal from the other thread.
4. **Timed Waiting:** A runnable thread enters into this state for a specific time interval and then transitions back when the time interval expires or the event the thread was waiting for occurs.
5. **Terminated (Dead)**: A thread enters into this state after completing its task.

**Types of execution in OS**:
1.  **Concurrent Execution:** This occurs when a processor is successful in switching resources between threads in a multithreaded process on a single processor.
2.  **Parallel Execution:** This occurs when every thread in the process runs on a separate processor at the same time and in the same multithreaded process


**Benefits**:
- Multithreading can improve the performance and efficiency of a program by utilizing the available CPU resources more effectively. Executing multiple threads concurrently, it can take advantage of parallelism and reduce overall execution time.
- Multithreading can enhance responsiveness in applications that involve user interaction. By separating time-consuming tasks from the main thread, the user interface can remain responsive and not freeze or become unresponsive.
- Multithreading can enable better resource utilization. For example, in a server application, multiple threads can handle incoming client requests simultaneously, allowing the server to serve more clients concurrently.
- Multithreading can facilitate better code organization and modularity by dividing complex tasks into smaller, manageable units of execution. Each thread can handle a specific part of the task, making the code easier to understand and maintain.

**Drawbacks**:
- If the **locking mechanisms** is not used properly, while investigating data access issues there is a chance of problems arising like **data inconsistency** and **dead-lock**.
- If many threads try to access the same data, then there is a chance that the situation of **thread starvation** may arise. Resource contention issues are another problem that can trouble the user.
- Display issues may occur if threads lack coordination when displaying data.


#### Types of multi-threading
##### Interleaved / Temporal multithreading
###### Coarse-grained multithreading
The simplest type of multithreading occurs when one thread runs until it is **blocked** by an event that normally would create a **long-latency stall**. Such a stall might be a **cache miss** that has to access **off-chip memory**, which might take **hundreds of CPU cycles** for the data to return. Instead of waiting for the stall to resolve, a threaded processor would **switch execution** to another thread that was **ready to run**. Only when the data for the previous thread had **arrived**, would the previous thread be **placed back on the list** of ready-to-run threads.

Below is an example for coarse-grained multithreading: 
1. `Cycle i`: instruction j from thread A is issued.
2. `Cycle i + 1`: instruction j + 1 from thread A is issued.
3. `Cycle i + 2`: instruction j + 2 from thread A is issued, which is a load instruction that misses in all caches.
4. `Cycle i + 3`: thread scheduler invoked, switches to thread B.
5. `Cycle i + 4`: instruction k from thread B is issued.
6. `Cycle i + 5`: instruction k + 1 from thread B is issued.

Switching from one thread to another means the hardware switches from using one register set to another. To achieve this goal, the hardware for the program visible registers, as well as some processor control registers (such as the program counter), is replicated. For example, to quickly switch between two threads, the processor is built with two sets of registers.

Additional hardware support for multithreading allows thread switching to be done in one CPU cycle, bringing performance improvements. Also, additional hardware allows each thread to behave as if it were executing alone and not sharing any hardware resources with other threads, minimizing the amount of software changes needed within the application and the operating system to support multithreading.

###### Fine-grained multithreading 
The purpose of fine-grained multithreading (first called barrel processing) is to remove all **data dependency stalls** from the **execution pipeline**. Since one thread is **relatively independent** from other threads, there is less chance of one instruction in one pipelining stage needing an output from an older instruction in the pipeline. Conceptually, it is similar to preemptive multitasking used in operating systems; an analogy would be that the **time slice** given to each active thread is one CPU cycle.

Below is an example for fine-grained multithreading: 
1. `Cycle i + 1`: an instruction from thread B is issued.
2. `Cycle i + 2`: an instruction from thread C is issued.

In addition to the hardware costs discussed in the block type of multithreading, interleaved multithreading has an additional cost of each pipeline stage tracking the thread ID of the instruction it is processing. Also, since there are more threads being executed concurrently in the pipeline, shared resources such as caches and TLBs need to be larger to avoid thrashing between the different threads.
##### Simultaneous multithreading
The most advanced type of multithreading applies to superscalar processors. Whereas a normal superscalar processor issues multiple instructions from a single thread every CPU cycle, in simultaneous multithreading (SMT) a superscalar processor can issue instructions from multiple threads every CPU cycle. Recognizing that any single thread has a limited amount of instruction-level parallelism, this type of multithreading tries to exploit parallelism available across multiple threads to decrease the waste associated with unused issue slots.

Below is an example for simultaneous multithreading: 
1. `Cycle i`: instructions j and _j_ + 1 from thread A and instruction k from thread B are simultaneously issued.
2. `Cycle i + 1`: instruction _j_ + 2 from thread A, instruction _k_ + 1 from thread B, and instruction m from thread C are all simultaneously issued.
3. `Cycle i + 2`: instruction _j_ + 3 from thread A and instructions _m_ + 1 and _m_ + 2 from thread C are all simultaneously issued.

To distinguish the other types of multithreading from SMT, the term "temporal multithreading" is used to denote when instructions from only one thread can be issued at a time.

In addition to the hardware costs discussed for interleaved multithreading, SMT has the additional cost of each pipeline stage tracking the thread ID of each instruction being processed. Again, shared resources such as caches and TLBs have to be sized for the large number of active threads being processed.


##### Lock / semaphore mechanism
To prevent data corruption / memory leaks, a well-defined lock (or semaphore) mechanism should be used. The basic idea is when a core or thread is accessing the resources, it put a lock to the resources so other core or thread cannot access it until the accessing session of the core is finished and the lock has been removed. 



#### Implementation
