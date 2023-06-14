
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

