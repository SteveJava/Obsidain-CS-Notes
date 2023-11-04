> Parallel Programming: using additional computational resources to produce an answer faster
## What is a Process?

- When a program runs, it becomes a process with its own address

- A process is represented by its **code, data** and the **state of the machine registers**
- The data of the process is divided into:
	- global variables and local variables
	- organised as a **stack**

It is hard to obtain parallelism or concurrency with separate processes...
...so operating systems created [[Threads]]

## What is a parallel program?

- The [[Shared memory]] model has multiple explicit threads running concurrently

Threads can:
- perform **multiple computations** in parallel
- perform separate **simultaneous activities**
- **communicate easily** and **implicitly** with each other through [[shared memory]]

## How to write parallel programs

- Java uses the [[shared memory]] programming model
- Use the [[Thread class]] or [[Runnable interface]]

- There are other [[programming models]].

- New primitives are required to enable:
	- running **multiple** operations at **once** (Java has concurrent threads)
	- share **data** between operations (Java uses [[Shared memory]])
	- **co-ordinate** (aka *synchronise*) **operations** 