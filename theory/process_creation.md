# 1. Explain the Concept of Process Creation in Operating Systems

## Definition
A **process** is nothing but a *program in execution*.  
It has its own **memory**, **CPU time**, **resources**, and **execution state**.

## Process Creation
In general, **process creation** means that the operating system creates a new process and allocates system resources to it so that it can execute independently.

A new process is created by an existing process called the **parent process**, and their relationship is known as the **parent–child relationship**.

## Process Creation Using `fork()`
In C, a new process is typically created using the `fork()` system call:


## after fork 2 process exists one is parent one(returns child pid) another one is child process (returns 0)
