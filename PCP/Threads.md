# What is a Thread?

> A thread is the smallest unit of execution to which the CPU allocates time

- A process is given internal concurrency with multiple *lightweight processes* or ***threads***.

- A process with multiple threads of control has multiple stacks (one for each thread) and access to **shared memory** too

***
# How do threads run?

> As the programmer, you create threads

- The **Operating System scheduler** determines how and when threads are run on the available processors

## Asynchrony

> Asynchrony means threads are subject unpredictable delays

- Cache misses (short): occur between local cache and [[Shared memory]] 
- Page faults (long): occur between [[Shared memory]] and disk of the PC
- Scheduling quantum finished (really long)

___
# Java is always *multi-threaded*

> In Java, every program has more than one thread

The Java Virtual Machine executes as a **process** under the operating system and supports **multiple threads**

- Start with one thread, called the *main thread*, which creates additional threads
- System threads perform **garbage collection** and **signal handling** 
- Threads compile Java byte-code into machine-level instructions

___
# Basic Threads in Java

All threads are associated with an instance of  ```java.lang.Thread```
### Two basic strategies for using thread objects

1. **Instantiate Thread** each time the application needs to initiate an asynchronous task
2. Pass the applications task to an **executor** ```java.util.concurrent //Launch and manage threads```

### Two ways to create basic threads
1. Extend/subclass the [[Thread class]]  ``` java.lang.Thread;```
2. Implement the [[Runnable interface]] in an object and pass that to a thread's constructor ``` java.lang.Runnable;```

___
## Start() vs Run()

- see [[Thread class]] and [[Runnable interface]] for code examples

1. `start()` method
	- `start()` creates threads that run concurrently in your program
	- This thread runs independently of the calling thread, allowing for true parallelism
	- The calling thread can continue its execution without waiting for the new thread to finish

1. `run()` method
	- when you call `run()`, the method runs synchronously in the same thread as the caller
	- The calling thread will wait for the `run()` method to complete its execution before continuing further

- If `c.start()` is changed to `c.run()` in a parallel program, parallelism will be lost.
- Threads will be created an run sequentially (one after another)

- The `run()` method will execute synchronously in the same thread as the caller, meaning it won't run concurrently with the rest of your program. 

- This can significantly impact the program's performance, especially if the `run()` method involves time-consuming tasks, as it will block the execution of the rest of your program until it completes.

___
## Types of threads

1. User threads
	- Regular threads created by the application
	- The JVM will wait for all user threads to execute before terminating the program
	- Can create them by instantiating `Thread` objects and calling `start()`

2. Daemon threads
	- threads that run in the background
	- used for tasks that do not require program to wait for them to complete
	- Once all user threads are done executing,  program is terminated
