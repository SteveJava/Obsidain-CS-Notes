- In Java, the `Thread` class is a fundamental part of the multithreading capabilities of the Java language. It is part of the `java.lang` package and provides methods to create and control [[threads]] of execution in a Java program. 
- Threads are used to enable multiple operations to be performed concurrently within a single program.

Here are some key aspects of the `Thread` class:

### Creating Threads:

1. **Extending the `Thread` class:**
   - You can create a new thread by creating a subclass of the `Thread` class and overriding its `run()` method. The `run()` method contains the code that will be executed when the thread is started.
   - Example:
   ```java
   class MyThread extends Thread {
       public void run() {
           // Code to be executed by the thread
       }
   }
   ```

2. **Implementing the [[Runnable interface]]:**
   - Alternatively, you can implement the `Runnable` interface, which defines a `run()` method. Then, you can create a `Thread` object by passing an instance of your class that implements `Runnable` to the `Thread` constructor.
   - Example:
   ```java
   class MyRunnable implements Runnable {
       public void run() {
           // Code to be executed by the thread
       }
   }
   ```

### Starting Threads:

- After creating a `Thread` object, you can start the thread's execution by calling its `start()` method. This method internally calls the `run()` method, allowing the code inside `run()` to run concurrently in a separate thread.

### Thread Lifecycle:

- Threads have different states, including **New**, **Runnable**, **Blocked**, **Waiting**, **Timed Waiting**, and **Terminated**. The `Thread` class provides methods to query and control the state of a thread.

### Thread Synchronisation:

- When multiple threads access shared resources, synchronisation mechanisms such as `synchronized` methods or blocks are used to ensure data consistency and prevent race conditions.

### Example Usage:

```java
public class Main {
    public static void main(String[] args) {
        // Creating a thread by extending the Thread class
        MyThread myThread = new MyThread();
        myThread.start(); // Start the thread

        // Creating a thread by implementing the Runnable interface
        Thread thread = new Thread(new MyRunnable());
        thread.start(); // Start the thread
    }
}
```

In this example, `MyThread` is a subclass of `Thread`, and `MyRunnable` implements the `Runnable` interface. Both threads are started using the `start()` method, allowing their `run()` methods to execute concurrently. Threads provide a way to achieve parallelism and can be used for various tasks in Java applications.

___
### Thread methods
The `Thread` class in Java provides several methods that are commonly used for working with threads. Here are some of the basic methods of the `Thread` class:

1. **`start()`:**
   - This method starts the execution of the thread. The `run()` method of the thread will be invoked concurrently in a separate new thread.

   Example:
   ```java
   Thread myThread = new Thread(new MyRunnable());
   myThread.start();
   ```

2. **`run()`:**
   - This method contains the code that constitutes the new thread. It is invoked when `start()` is called. You should override this method when you extend the `Thread` class or implement the `Runnable` interface.

   Example:
   ```java
   class MyThread extends Thread {
       public void run() {
           // Code to be executed by the thread
       }
   }
   ```

3. **`sleep(long millis)`:**
   - This method causes the currently executing thread to sleep (temporarily cease execution) for the specified number of milliseconds.

   Example:
   ```java
   try {
       Thread.sleep(1000); // Sleep for 1 second
   } catch (InterruptedException e) {
       // Handle the exception
   }
   ```

4. **`join()`:**
   - This method makes the current thread wait until the thread on which it's called completes its execution.

   Example:
   ```java
   Thread thread = new Thread(new MyRunnable());
   thread.start();
   try {
       thread.join(); // Wait for the thread to finish
   } catch (InterruptedException e) {
       // Handle the exception
   }
   ```

5. **`isAlive()`:**
   - This method checks if the thread is still alive. A thread is alive if it has been started and has not yet died.

   Example:
   ```java
   Thread myThread = new Thread(new MyRunnable());
   myThread.start();
   boolean isAlive = myThread.isAlive(); // Check if the thread is alive
   ```

6. **`interrupt()`:**
   - This method interrupts the thread, causing it to exit its current state and either throw an `InterruptedException` or complete its execution, depending on the context.

   Example:
   ```java
   Thread myThread = new Thread(new MyRunnable());
   myThread.start();
   // ...
   myThread.interrupt(); // Interrupt the thread
   ```

These are some of the basic methods of the `Thread` class. There are more advanced methods and techniques for thread synchronisation and coordination in Java, but these methods provide a good starting point for working with threads.

### Example code

```Java
public class HelloThreadMethods extends java.lang.Thread { 
	private int i; 
	private boolean sleepy; // sleepy for slower execution
	private boolean polite; // polite to yield to other threads

	HelloThreadMethods(int i) { // Default constructor
		this(i, false, false); // set sleepy and polite to false 
	}

	HelloThreadMethods(int i, boolean slpy, boolean plt) { 
		this.i = i;
		sleepy=slpy; polite=plt;  
	}

	public void run() {
		// Executed when a thread is started  
		System.out.println("Thread " + i + " says hi"); 
		if(polite) yield(); // thread more likely to finish last, Allow  
							// other threads to run 
		if(sleepy)
			try {  
				System.out.println("Thread " + i + " snoozing");
				sleep(1000); // Make current thread sleep 1000 millis
			} catch (InterruptedException e) {  
			// TODO Auto-generated catch block
				e.printStackTrace(); 
			}
			// If applicable, print another message
		System.out.println("Thread " + i + " says bye"); 
	}

	public static void main(String[] args) throws InterruptedException { 
		int noThrds=10;
		// Threads array  
		HelloThreadMethods [] thrds = new HelloThreadMethods[noThrds]; 
		for(int i=0; i < noThrds; ++i) {
			// First thread is sleepy and not poolite
			if (i==0) thrds[i] = new HelloThreadMethods(i,true,false); 
			else if(i==(noThrds-1)) 
				thrds[i] = new HelloThreadMethods(i,false,true); 
				//last thread is polite and polite
				// All other threads are neither sleepy nor polite
			thrds[i].start();
		}  
		for(int i=0; i < noThrds; ++i) {
			thrds[i].join(); //main thread waits for HelloThread i
					// and all other threads to finis thier join()
		}
		System.out.println("we are all done"); 
	}
}
```