
**Out of the 5-6 states you can find under ps, which two takes up system memory?**

Among the various states found under the ps command, two states that consume system memory are:

"R" (Running): The "R" state represents processes that are actively running and executing in the CPU. These processes consume system memory while executing their code, storing variables, and maintaining data structures in memory.

"S" (Sleeping): The "S" state indicates processes that are sleeping or waiting for an event to occur. While in the sleeping state, these processes still consume system memory to retain their context and data. Although they are not actively using CPU resources, their memory allocations remain intact.

Both the "R" and "S" states represent processes that occupy system memory, but they differ in terms of their CPU utilization. The "R" state involves actively executing processes, while the "S" state refers to processes that are temporarily idle and waiting for a specific event.


**Name three states a process can be in**


Running State: In this state, the process is actively executing instructions on the CPU and making progress towards its completion.

Waiting State: Also known as the Blocked State or the Suspended State, a process enters this state when it is unable to proceed further until a certain event occurs. For example, a process may be waiting for user input, waiting for a resource to become available, or waiting for a signal from another process.

Ready State: When a process is loaded into the main memory and is waiting for the CPU to execute it, it is said to be in the Ready State. Multiple processes in the Ready State are usually kept in a queue, and the CPU scheduler selects one of them to enter the Running State based on the scheduling algorithm employed by the operating system.


What are zombie processes and how are they reaped ? 

A zombie process, also known as a defunct process, is a term used in computer operating systems to describe a process that has completed execution but still has an entry in the process table. In other words, it's a process that has finished its execution but hasn't been fully removed from the system. When a process finishes executing, it sends a signal to its parent process to notify that it has completed its task. The parent process then typically calls the `wait()` system call to collect the exit status of the child process and free up the resources associated with it. However, if the parent process fails to call wait(), the terminated child process remains in the process table as a zombie process. If a parent dies before its child then the init process adopts it and then periodically sends the wait call to reap any child processes or zombie processes.



**How to make a process a service ?**
To make a process a service, you need to create a systemd service file. A systemd service file is a text file that tells systemd how to start, stop, and manage the service. The file has three sections:

* **[Unit]** section: This section contains general information about the service, such as its name, description, and dependencies.
* **[Service]** section: This section contains the commands that systemd should use to start, stop, and manage the service.
* **[Install]** section: This section tells systemd where to install the service.

Here is an example of a systemd service file for a simple service that prints "Hello, world!" to the console:

```
[Unit]
Description=Simple service that prints "Hello, world!"
After=network.target

[Service]
ExecStart=/bin/echo "Hello, world!"

[Install]
WantedBy=multi-user.target
```

To create a systemd service file, you can use a text editor, such as Nano or Vim. Once you have created the file, you need to save it with the .service extension. For example, if you named your file `myservice.service`, you would save it as `/etc/systemd/system/myservice.service`.

Once you have created the service file, you need to enable it. You can do this by running the following command:

```
systemctl enable myservice
```

You can start the service by running the following command:

```
systemctl start myservice
```

You can stop the service by running the following command:

```
systemctl stop myservice
```

You can check the status of the service by running the following command:

```
systemctl status myservice
```

For more information about systemd service files, you can refer to the systemd documentation:

<https://www.freedesktop.org/software/systemd/man/systemd.unit.html>


**What are the targets that systemd aims to start**
When a Linux system is booted using systemd as the init system, several targets are typically started in a sequential order. These targets represent different stages of the boot process and ensure that essential services and functionalities are initialized correctly. The following are the common targets started during the boot process:

1. `basic.target`: This is the first target started by systemd and represents the minimal environment needed for the system to function. It includes basic system initialization, such as setting the hostname, configuring the system clock, and enabling essential system services.

2. `multi-user.target`: Once the basic.target is reached, the multi-user.target is activated. This target brings the system to a state where multiple users can log in and perform various tasks. It starts essential system services required for multi-user operations, including networking, system logging, and other core services.

3. `graphical.target`: If the system is configured for a graphical user interface (GUI), the graphical.target is activated after the multi-user.target. It starts the display manager, window manager or desktop environment, and the X Server or Wayland compositor, as mentioned in the previous response. It provides a full-fledged graphical environment for the user.

4. `rescue.target` or `emergency.target`: These targets are typically used for system recovery or troubleshooting purposes. If the system encounters critical errors during boot or if the administrator explicitly chooses to enter rescue or emergency mode, these targets are activated. They provide a minimal environment with limited services, allowing the user to fix any issues or perform system maintenance tasks.

In addition to these primary targets, systemd also starts various other targets and services as dependencies, ensuring proper initialization and management of system components. These can include targets related to networking, storage, logging, device management, and more.

It's important to note that the actual list and naming of targets can vary depending on the specific Linux distribution and configuration. System administrators can also create custom targets to meet specific requirements or establish dependencies between services.