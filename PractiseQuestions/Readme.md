
**Out of the 5-6 states you can find under ps, which two takes up system memory?**

Among the various states found under the ps command, two states that consume system memory are:

"R" (Running): The "R" state represents processes that are actively running and executing in the CPU. These processes consume system memory while executing their code, storing variables, and maintaining data structures in memory.

"S" (Sleeping): The "S" state indicates processes that are sleeping or waiting for an event to occur. While in the sleeping state, these processes still consume system memory to retain their context and data. Although they are not actively using CPU resources, their memory allocations remain intact.

Both the "R" and "S" states represent processes that occupy system memory, but they differ in terms of their CPU utilization. The "R" state involves actively executing processes, while the "S" state refers to processes that are temporarily idle and waiting for a specific event.


**Name three states a process can be in**


Running State: In this state, the process is actively executing instructions on the CPU and making progress towards its completion.

Waiting State: Also known as the Blocked State or the Suspended State, a process enters this state when it is unable to proceed further until a certain event occurs. For example, a process may be waiting for user input, waiting for a resource to become available, or waiting for a signal from another process.

Ready State: When a process is loaded into the main memory and is waiting for the CPU to execute it, it is said to be in the Ready State. Multiple processes in the Ready State are usually kept in a queue, and the CPU scheduler selects one of them to enter the Running State based on the scheduling algorithm employed by the operating system.




