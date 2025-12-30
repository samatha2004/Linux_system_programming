```md
# lsp-threads
===========

## 1) why a stack grows?

when we create a new thread the memory will be created in the stack called stack area

Every function call creates a stack frame, so the stack grows.

When functions return, stack frames are removed, so the stack shrinks.

---

## 2) what segments are shared by multiple threads within a process?

Segments shared by multiple threads within a process:

Text (Code) segment

Data segment

BSS segment (uninitialized global & static variables)

Heap segment

Not shared:

Stack (each thread has its own)

Registers

---

## 3) can you fetch the thread entry point return value in your main thread ?

The thread entry function can return a value using return or pthread_exit().

The main thread (or any other thread) can collect that return value using pthread_join().

---

## 4) what happens when main() is invoked ?

When the main() function is invoked, memory is created in RAM and divided into memory segments.  
At the system level, in ARM architecture, function or subroutine invocation is translated into a BL (Branch with Link) instruction.  
The function name represents the base address of the function, which is the address of the first instruction stored in the text segment.  
When this base address is loaded, it is copied into the Program Counter (PC), and execution of main() begins.

---

## 5) What happens when the CPU stops executing a process?

When the CPU time slice expires, a timer interrupt occurs.  
The OS saves the CPU context (registers, program counter, stack pointer) of the current process.  
A context switch is performed, and the scheduler selects the next ready process.  
The CPU then restores the saved context of the next process and resumes execution.

---

## 6) During the context switch, which instruction will be used to copy the contents of cpu register to your context area of PCB?

a) using the STR instruction

---

## 7) How do you create separate process?

by using the fork() system call we can create a separate process

---

## 8) How do server create separate thread?

by using the library function pthread_create() which is present in libpthread.so library

---

## 9) Advantage of Thread over process?

Thread creation consumes less memory, mainly only the stack area (around 2 MB).

Thread creation is much faster than process creation.

Sharing of information is easy in threads within the same process.

Creation of a new process consumes a large amount of memory, sometimes up to 4 GB of virtual memory, depending on execution.

Process creation using fork() is expensive because it involves duplication of process attributes and information present in the PCB.

Process creation is slower compared to threads.

Process information like Data, BSS, and Heap can be easily shared between multiple threads within a process.

Context switching between threads is faster compared to processes.

Parent and child processes have separate code and data segments, so IPC mechanisms are required for sharing.

Context switching between processes is slower than between threads.

-----------------

## 10) Advantage of process over Thread?

In a process, multiple processes do not share the same memory by default.

In threads, multiple threads share the same memory, so sharing information is easy.

When multiple threads access shared memory, synchronization and data update issues can occur.

These synchronization issues can be avoided using proper mechanisms (like locks).

----------------

## 11) Why do these problems occur?

These problems occur when multiple threads within the same process try to access and modify the same global variable present in the data segment at the same time.

---------------

## 12) How much CPU time is given to user space threads and kernel space threads?

User space threads do not get CPU time directly from the kernel.  
The kernel schedules only the process, and the process internally manages CPU time among its user space threads.

Kernel space threads get CPU time directly from the kernel, equal to that of a normal process, since each kernel thread is treated like a schedulable entity.

-------------

## 13) what is an api ?

api is application programming interface it acts like a bridge between our program and the os and the libraries..  
API (Application Programming Interface) is a set of functions and rules that allows one software component to communicate with another.

fork();  
read();  
write();

These are APIs that let your program talk to the operating system.

---

## 14) explain posix and system-v difference ?

Key difference in examples

POSIX APIs are usually library calls and easier to use.

System-V APIs are system calls, more low-level, and OS-specific.

POSIX Examples

Threads: pthread_create(), pthread_join()

Shared Memory: shm_open(), mmap()

Semaphores: sem_open(), sem_wait(), sem_post()

Message Queues: mq_open(), mq_send(), mq_receive()

Signals: sigaction(), kill()

System-V Examples

Shared Memory: shmget(), shmat(), shmdt()

Semaphores: semget(), semop(), semctl()

Message Queues: msgget(), msgsnd(), msgrcv()

POSIX is a standard that defines a common set of APIs for processes, threads, and IPC to ensure portability across Unix-like systems.  
System-V is a Unix implementation that provides IPC mechanisms like message queues, shared memory, and semaphores using system-call based APIs.

------------

## 15) points to remember when we use the mutex locs to protect the critical section

Same mutex should be used in all threads that access the critical section.

Mutex locks should be created as global variables, not local.

If a mutex is created locally inside a thread function, it will reside on that thread’s stack.

Even if multiple mutexes have the same name locally, they are different objects in memory, so they do not ensure mutual exclusion.

Separate mutex must be used for each global variable that needs protection.

---

## 16) mutex_lock achieves what ?

mutex_lock() ensures that only one thread executes a critical section at a time, providing thread safety.  
Protects shared/global variables.

Prevents data corruption due to concurrent access.

Works with mutex_unlock() to release the lock for other threads.

---

## 17) difference between mutex and semaphore ?

Mutex: Counter can have only 0 or 1.

Semaphore: Counter can have 0, 1, or any positive integer.

Mutex: Used to protect shared memory/resources between threads.

Semaphore: Used to protect resources between threads or processes.

Mutex: Only the thread that locks it can unlock it.

Semaphore: Any thread or process can unlock it.

Mutex: Suitable for thread synchronization within a process.

Semaphore: Suitable for synchronization between threads or processes.

---

## 18) varients of pthread_mutex_lock()

Variants of pthread mutex locks

pthread_mutex_lock()

Changes the mutex state from unlocked → locked.

Counter state changes from 1 → 0.

Blocks indefinitely until the mutex is unlocked by the owner.

pthread_mutex_trylock()

Changes state from unlocked → locked if available.

Does not block if mutex is already locked.

Returns an error EBUSY if the mutex is locked.

pthread_mutex_timedlock()

Changes state from unlocked → locked.

Blocks only for a specified time period.

Returns an error if the mutex is not available within the timeout.

-----------

## 19) pthread_mutex_timedlock()

Working of pthread_mutex_timedlock()

Suppose a thread calls pthread_mutex_timedlock() with a timeout of t ms.

The thread waits to acquire the mutex for that specified time.

If the mutex is unlocked within the timeout, the thread acquires the lock and enters the critical section.

If the mutex is not unlocked within the timeout, pthread_mutex_timedlock() returns an ETIMEOUT error.

On receiving the error, the thread skips the critical section and continues execution.

-----------

## 20) explain the compilation of a thread

A thread is compiled like a normal program, but the -pthread flag links the pthread library, enabling thread creation and management at runtime.

gcc program.c -o program -pthread
```
