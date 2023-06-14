Threads are genereally lightweight execution unit inside a process. When an interviewer deep dives into threads they generally expect you to know the following

* How are threads created, can you tell me some of the commands

* How do threads communicate with each other.

* Is there an upper limit on the thread pool

* What happens when there is a race condition for data between threads

* What are virtual threads ? 

Let us deep dive into the thread world 

Threads in Linux can be created using various approaches. The POSIX Threads (pthread) library provides functions like `pthread_create()` for thread creation. The `clone()` system call offers a low-level interface for more control over thread creation. OpenMP is an API supporting multi-threading with compiler directives.


Threads communicate with each other by sharing data through shared memory and utilizing synchronization mechanisms to coordinate access to that shared data. Here are some common methods of thread communication:

**Shared Memory:** Threads can communicate by accessing shared variables or data structures residing in shared memory. Multiple threads can read and write to these shared locations. However, to ensure data integrity and avoid race conditions, synchronization mechanisms like mutexes, semaphores, or read-write locks should be used to coordinate access to shared data.

**Message Passing:** Threads can communicate by sending messages to each other. In this approach, threads use explicit message passing mechanisms provided by the programming language or libraries. Threads can exchange information by sending messages through channels, queues, or other communication constructs. Synchronization mechanisms may still be necessary to coordinate message sending and receiving.

**Condition Variables:** Threads can use condition variables to synchronize their activities and communicate based on specific conditions. A condition variable allows a thread to wait until a particular condition is met. When that condition becomes true, another thread can signal the condition variable to wake up the waiting thread and proceed with its execution.



**What is a kernel thread and do they share memory like user level threads ?**
Kernel threads, by default, do not share memory in the same way as user-level threads. Each kernel thread typically has its own separate memory space, including its stack, registers, and other kernel-level data structures. This isolation ensures that one kernel thread cannot directly access or modify the memory of another kernel thread.

However, it's important to note that kernel threads can still indirectly share memory through shared resources, such as files, sockets, or shared memory segments. These shared resources allow communication and data exchange between kernel threads, but the memory sharing is typically mediated by the operating system through appropriate synchronization mechanisms.

On the other hand, user-level threads, which are managed by a user-space runtime library or framework, can share memory more directly. They operate within a single process and share the same memory space, allowing them to access and modify shared variables and data structures without the need for explicit synchronization mechanisms.

In summary, while kernel threads do not share memory in the same way as user-level threads, they can still communicate and exchange data through shared resources provided by the operating system. User-level threads, on the other hand, have more direct access to shared memory within a process, enabling easier and faster communication between threads.
