
The procfs directory stores information about a particular process

/proc/[pid]/cmdline: This file contains the command-line arguments passed to the process at the time of its execution. It is a null-separated list of arguments.

/proc/[pid]/cwd: This symbolic link points to the current working directory of the process.

/proc/[pid]/environ: This file contains the environment variables of the process.

/proc/[pid]/exe: This symbolic link points to the executable file of the process.

/proc/[pid]/fd: This directory contains symbolic links to file descriptors opened by the process. Each symbolic link corresponds to a specific file descriptor.

/proc/[pid]/maps: This file shows the memory mapping of the process, including the ranges of memory regions allocated to different parts of the process.

/proc/[pid]/status: This file provides various status information about the process, such as process ID, parent process ID, memory usage, CPU usage, and more.

/proc/[pid]/stat: This file contains various statistics and information about the process, including process ID, parent process ID, state, memory usage, and CPU time.

/proc/[pid]/io: This file displays input/output statistics related to the process.

/proc/[pid]/sched: This directory contains scheduling-related information about the process, such as scheduling policy, priority, and more.


**The various states a process can be in**

Certainly! Here's an explanation of the different process states you mentioned:

1. `TASK_INTERRUPTIBLE`: This state indicates that the process is sleeping or blocked, waiting for a specific condition to exist. The process is not actively running and is not eligible to run until the condition it is waiting for occurs. If the condition is met or if the process receives a signal, it transitions to the `TASK_RUNNING` state and becomes runnable, ready to be scheduled for execution. In this state, the process can be awakened prematurely by a signal, causing it to become runnable.

2. `TASK_UNINTERRUPTIBLE`: This state is similar to `TASK_INTERRUPTIBLE` in that the process is sleeping or blocked, waiting for a specific condition. However, unlike `TASK_INTERRUPTIBLE`, a process in the `TASK_UNINTERRUPTIBLE` state does not wake up and become runnable if it receives a signal. This state is used in situations where the process must wait without interruption or when the expected event is anticipated to occur quickly. Since the process does not respond to signals in this state, `TASK_UNINTERRUPTIBLE` is less commonly used compared to `TASK_INTERRUPTIBLE`.

3. `TASK_ZOMBIE`: This state indicates that the process has terminated its execution but its parent process has not yet issued a `wait4()` system call to collect the exit status of the child process. The process descriptor of the terminated process is retained in case the parent process wants to access it. Once the parent process calls `wait4()`, the process descriptor is deallocated, and the process is considered fully terminated.

4. `TASK_STOPPED`: When a process receives certain signals like `SIGSTOP`, `SIGTSTP`, `SIGTTIN`, or `SIGTTOU`, its execution is stopped, and it enters the `TASK_STOPPED` state. In this state, the process is not running, nor is it eligible to run. It remains in a stopped state until it receives a corresponding signal that allows it to resume execution or if it is being debugged and receives any signal.

5. `TASK_RUNNING`: This state indicates that the process is runnable. It is either currently running on a CPU or waiting in a runqueue to be scheduled for execution. For a process executing in user-space, this is the only possible state. It can also apply to a process in kernel-space that is actively running.

These process states reflect the different stages and conditions that a process can be in during its lifecycle. They help the operating system manage and schedule processes effectively, ensuring efficient resource utilization and responsiveness to events and signals.

**How to daemonize a process ?**


To daemonize a process means to detach it from the controlling terminal, allowing it to run in the background as a daemon. Here's a general outline of the steps involved in daemonizing a process:

1. Fork and Terminate the Parent:
   - The parent process forks a child process and then terminates itself. This step ensures that the child process becomes an orphan and gets adopted by the init process (usually with PID 1).

2. Set File Permissions:
   - In the child process, adjust the file permissions using `umask()` to control the access permissions for files created by the daemon.

3. Create New Session and Process Group:
   - Call `setsid()` to create a new session and process group for the child process. This detaches the process from the controlling terminal.

4. Change Working Directory:
   - Use `chdir()` to change the working directory to a suitable location (e.g., `/`) to avoid interfering with the filesystem unmounting.

5. Close Standard File Descriptors:
   - Close the standard file descriptors (stdin, stdout, stderr) by calling `close()` on file descriptors 0, 1, and 2. This ensures that the daemon doesn't inadvertently read from or write to the terminal.

6. Redirect Standard File Descriptors:
   - Optionally, redirect the standard file descriptors to `/dev/null` or other log files if necessary.

7. Perform Initialization Tasks:
   - Perform any initialization tasks required by the daemon, such as opening log files or establishing network connections.

8. Enter the Daemon Main Loop:
   - Enter the main loop of the daemon, where it performs its intended functionality or waits for events to handle.

