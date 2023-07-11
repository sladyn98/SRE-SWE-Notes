Fork() : The fork call is used to  duplicate the current process, the new process  identical in almost every way except that it has its own PID. The return value of the function  fork distinguishes the two processes, zero is returned in the child and PID of child in parent process.

The `exec()` family of functions in Unix-like operating systems is used to replace the current process image with a new program. They load a new executable file into the current process and start its execution from the entry point. The `exec()` functions provide different ways to specify the program name and its arguments. Here are the variations of `exec()` functions along with examples:

1. `execl()`:
   - Prototype: `int execl(const char *path, const char *arg0, ..., (char *) NULL);`
   - Arguments: Takes the program name and its arguments as separate arguments, with the last argument being a NULL pointer to indicate the end of the argument list.
   - Example:

   ```c
   #include <unistd.h>
   
   int main() {
       execl("/bin/ls", "ls", "-l", NULL);
       return 0;
   }
   ```

2. `execlp()`:
   - Prototype: `int execlp(const char *file, const char *arg0, ..., (char *) NULL);`
   - Arguments: Searches for the program in the directories listed in the `PATH` environment variable. Takes the program name and its arguments as separate arguments, with the last argument being a NULL pointer.
   - Example:

   ```c
   #include <unistd.h>
   
   int main() {
       execlp("ls", "ls", "-l", NULL);
       return 0;
   }
   ```

3. `execle()`:
   - Prototype: `int execle(const char *path, const char *arg0, ..., (char *) NULL, char *const envp[]);`
   - Arguments: Similar to `execl()`, but it also allows specifying the environment variables for the new program using an array of strings. The last two arguments must be NULL pointers.
   - Example:

   ```c
   #include <unistd.h>
   
   int main() {
       char *envp[] = { "PATH=/usr/bin", NULL };
       execle("/bin/ls", "ls", "-l", NULL, envp);
       return 0;
   }
   ```

4. `execv()`:
   - Prototype: `int execv(const char *path, char *const argv[]);`
   - Arguments: Takes the program name and its arguments as an array of strings, with the last element of the array being a NULL pointer.
   - Example:

   ```c
   #include <unistd.h>
   
   int main() {
       char *argv[] = { "ls", "-l", NULL };
       execv("/bin/ls", argv);
       return 0;
   }
   ```

5. `execvp()`:
   - Prototype: `int execvp(const char *file, char *const argv[]);`
   - Arguments: Searches for the program in the directories listed in the `PATH` environment variable. Takes the program name and its arguments as an array of strings, with the last element of the array being a NULL pointer.
   - Example:

   ```c
   #include <unistd.h>
   
   int main() {
       char *argv[] = { "ls", "-l", NULL };
       execvp("ls", argv);
       return 0;
   }
   ```

6. `execvpe()`:
   - Prototype: `int execvpe(const char *file, char *const argv[], char *const envp[]);`
   - Arguments: Similar to `execvp()`, but it also allows specifying the environment variables for the new program using an array of strings.
   - Example:

   ```c
   #include <

unistd.h>
   
   int main() {
       char *argv[] = { "ls", "-l", NULL };
       char *envp[] = { "PATH=/usr/bin", NULL };
       execvpe("ls", argv, envp);
       return 0;
   }
   ```

In these examples, the `exec()` functions are used to execute the `/bin/ls` program with the `-l` argument. Each variation provides a different way to specify the program name and its arguments, with some variants allowing customization of environment variables.

When the `exec()` function is called, the current process is replaced by the new program, and execution starts from the entry point of the new program. The current process's code, data, heap, stack, and environment are replaced by those of the new program.

These `exec()` functions are powerful tools for executing new programs within a process and are commonly used in scenarios such as launching external commands, implementing shells, or setting up custom environments for program execution.

Clone() : This system call in Unix-like operating systems is similar to `fork()`, but with additional flexibility. It allows for the creation of a new process that shares certain parts of its execution context with the calling process. Here's a detailed explanation of `clone()` along with an example:

The prototype for `clone()` is as follows:

```c
#include <sched.h>

int clone(int (*fn)(void *), void *child_stack, int flags, void *arg, ... /* pid_t *parent_tid, void *tls, pid_t *child_tid */ );
```

The `clone()` function takes several arguments:
- `fn`: A pointer to a function that will be executed by the child process. This function serves as the entry point for the child process's execution.
- `child_stack`: A pointer to the top of the child process's stack. The child process will use this stack for its function execution.
- `flags`: Specifies the specific behaviors and attributes for the clone operation, such as sharing of resources.
- `arg`: An argument passed to the `fn` function.

The `clone()` system call creates a new process, just like `fork()`, but with the ability to customize the execution context and behavior of the child process. The child process created by `clone()` shares parts of its execution context with the parent process, such as the memory space, file descriptors table, and signal handlers.



