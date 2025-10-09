### wait() system call
the wait system call is used by the parent process to ensure the parent process wait until the child 
process execution
it helps in syncronization
it also help the parent process to collect the exit code of the child process
Normally, both parent and child processes run independently after fork().
So — it’s not guaranteed who runs first (sometimes parent runs first, sometimes child).

To make sure the parent waits until the child finishes, we use the wait() system call.
This way, the parent doesn’t move ahead until the child completes, and it can also collect the child’s exit status (the exit code).
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    int pid = fork();

    if (pid == 0) {
        printf("Child process is running\n");
        sleep(2);
        printf("Child process finished\n");
    } else {
        wait(NULL);  // parent waits for child to finish
        printf("Parent process resumes after child ends\n");
    }

    return 0;
}
```
