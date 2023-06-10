# Signals

Signal handling is a pretty important topic in the linux kernel. Some of the important points to understand is 
a) Receiving Signal
b) How signals are represented
c) Signal Handling


## Receiving Signals

At some point the process is going to switch back into user mode from the kernel mode that is when the kernel checks for pending signals.

There are a few reasons why a signal cannot be sent while a process is in kernel mode.

* **Signals are delivered to user-mode processes.** When a signal is sent to a process, the kernel saves the current state of the process and then switches to another process. When the kernel switches back to the process that was sent the signal, the kernel delivers the signal to the process. However, when a process is in kernel mode, the kernel is not running user-mode code, so it cannot deliver signals to the process.
* **Sending a signal to a process in kernel mode could cause a crash.** When a process is in kernel mode, it has direct access to the hardware. If a signal is sent to a process in kernel mode, the signal could cause the process to crash. This is because the signal could interrupt the process while it is in the middle of a critical operation.
* **Sending a signal to a process in kernel mode could introduce security vulnerabilities.** If a signal could be sent to a process in kernel mode, an attacker could exploit this vulnerability to crash the process or gain unauthorized access to the process.

For these reasons, the kernel does not allow signals to be sent to processes that are in kernel mode.


## Interrupt vector Table

An interrupt vector is a small piece of data that tells the processor where to find the interrupt service routine (ISR) for a particular interrupt. When the processor receives an interrupt, it looks up the interrupt vector in the interrupt vector table (IVT) and jumps to the address stored in the vector. The ISR then handles the interrupt.

The IVT is a table that contains one entry for each interrupt that the processor can handle. Each entry in the IVT contains the address of the ISR for that interrupt. The IVT is typically located in the first 1024 bytes of memory.

When an interrupt occurs, the processor saves the current state of the processor, including the program counter, the processor registers, and the stack pointer. The processor then looks up the interrupt vector in the IVT and jumps to the address stored in the vector. The ISR then handles the interrupt.

When the ISR is finished handling the interrupt, it restores the processor to its previous state and then returns to the interrupted program.

The interrupt vector is a critical part of the interrupt handling process. It allows the processor to quickly and efficiently find the ISR for a particular interrupt. This ensures that interrupts are handled quickly and efficiently, which is important for maintaining system performance.

Here are some additional details about interrupt vectors:

* Interrupt vectors are typically 4 bytes long.
* The first two bytes of an interrupt vector contain the segment address of the ISR.
* The last two bytes of an interrupt vector contain the offset address of the ISR.
* The IVT is typically located in the first 1024 bytes of memory.
* The IVT is typically initialized by the operating system.
* The IVT can be modified by the operating system or by user-mode applications.

I hope this helps! Let me know if you have any other questions.

Signal Representation
The bits used to represent interrupt signals in the Linux kernel are the bits from 1 to 31 in the process's signal mask. The signal mask is a bitmask that indicates which signals are blocked for the process. If a signal is not blocked, the kernel will deliver it to the process when the process switches from kernel mode to user mode.

The following table shows the interrupt signals and the corresponding bits in the signal mask:

| Signal | Bit |
|---|---|
| SIGHUP | 1 |
| SIGINT | 2 |
| SIGQUIT | 3 |
| SIGILL | 4 |
| SIGABRT | 6 |
| SIGFPE | 8 |
| SIGKILL | 9 |
| SIGSEGV | 11 |
| SIGPIPE | 13 |
| SIGALRM | 14 |
| SIGTERM | 15 |
| SIGUSR1 | 30 |
| SIGUSR2 | 31 |

The kernel also uses the bits from 1 to 31 in the process's signal mask to determine which signals are blocked for the process when the process is interrupted. When a process is interrupted, the kernel saves the process's state and then switches to another process. When the kernel switches back to the interrupted process, it checks for any pending signals that were sent to the process while it was interrupted. If there are any pending signals, the kernel delivers them to the process. However, if the signal is blocked for the process, the kernel will not deliver it to the process.

The following table shows the interrupt signals that are blocked by default:

| Signal | Bit |
|---|---|
| SIGKILL | 9 |
| SIGSTOP | 17 |
| SIGTSTP | 18 |
| SIGTTIN | 19 |
| SIGTTOU | 20 |

The kernel can also block or unblock signals for a process using the `sigprocmask()` system call.

**Signal Handling**

We can use a signal handling method to handle certain signals or even override them. Some of the signals that you cannot override are the sigkill signal. 

```c

/* Signals/custom.c */
#include <stdio.h>
#include <signal.h>

void handler(int signal)
{
    printf("Signal %d Received.Kill me if you can\n",signal);
}

int main(int argc,char *argv[])
{
    signal(SIGINT,handler);
    printf("Put into while 1 loop..\n");
    while(1) { }
    printf("OK!\n");
    return 0;
}
```