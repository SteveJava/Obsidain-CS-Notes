- Fork/Join is a framework in Java that provides support for [[parallel programming]]. 
- It is particularly useful for recursive algorithms that can be divided into smaller tasks that can be executed independently and then combined to produce a result. 
- Fork/Join framework was introduced in Java 7 and is part of the `java.util.concurrent` package.

The key components of the Fork/Join framework are:

1. **[[ForkJoinPool]]:**
   - `ForkJoinPool` is a special type of [[Executor Service]] designed to work with Fork/Join tasks.
   - It manages a pool of worker [[threads]] that execute Fork/Join tasks.
   - Fork/Join tasks are submitted to the `ForkJoinPool` for execution.

2. **[[RecursiveTask]]\<T\>:**
   - `RecursiveTask<T>` is an abstract class in the Fork/Join framework that represents a task that returns a result of type `T`.
   - It extends `ForkJoinTask<T>` and provides a `compute()` method where the task logic is implemented.
   - `RecursiveTask<T>` is suitable for tasks that return a result.

3. **[[RecursiveAction]]:**
   - `RecursiveAction` is an abstract class in the Fork/Join framework that represents a task that does not return a result (returns `void`).
   - It also extends `ForkJoinTask<Void>` and provides a `compute()` method where the task logic is implemented.
   - `RecursiveAction` is suitable for tasks that perform an action without returning a result.

- The basic idea behind Fork/Join framework is to divide a large task into smaller subtasks until the subtasks are small enough to be executed directly. 
- These subtasks are executed in parallel by worker threads managed by the `ForkJoinPool`. After the subtasks are executed, their results are combined to produce the final result.

Here's a simple example of using the Fork/Join framework with a [[RecursiveTask]]:

```java
import java.util.concurrent.RecursiveTask;
import java.util.concurrent.ForkJoinPool;

public class SumTask extends RecursiveTask<Integer> {
    private static final int THRESHOLD = 10;
    private int[] array;
    private int start;
    private int end;

    public SumTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Integer compute() {
        if (end - start <= THRESHOLD) {
            // Perform the task directly if it's below the threshold
            int sum = 0;
            for (int i = start; i < end; i++) {
                sum += array[i];
            }
            return sum;
        } else {
            // Divide the task into smaller subtasks
            int mid = (start + end) / 2;
            SumTask leftTask = new SumTask(array, start, mid);
            SumTask rightTask = new SumTask(array, mid, end);

            // Fork the subtasks for parallel execution
            leftTask.fork();
            int rightResult = rightTask.compute();
            int leftResult = leftTask.join();

            // Combine the results from subtasks
            return leftResult + rightResult;
        }
    }

    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        ForkJoinPool forkJoinPool = new ForkJoinPool();
        int result = forkJoinPool.invoke(new SumTask(array, 0, array.length));
        System.out.println("Sum: " + result);
    }
}
```

- In this example, the `SumTask` class extends `RecursiveTask<Integer>` and calculates the sum of an array using the Fork/Join framework. 
- The task is divided into smaller subtasks until the size of the subtask is below the threshold (`THRESHOLD`). 
- Fork/Join framework manages the parallel execution of these subtasks, ensuring efficient utilisation of available CPU cores.

___
# Using Fork/Join

## 1. Create a Fork/Join Task

- Start by creating a class that extends [[RecursiveTask]] or [[RecursiveAction]] based on whether or not your task has a return value

- Override the compute() method with the logic of the task

- **ForkJoinTasks** (either `RecursiveAction` or `RecursiveTask`) are given to a **ForkJoinPool** (a pool of threads).

### Example code of Recursive Task

```Java
import java.util.concurrent.RecursiveTask;

public class MyTask extends RecursiveTask<Integer> {
    private static final int THRESHOLD = 10; // Define a threshold to determine when to stop dividing the task

    private int[] array;
    private int start;
    private int end;

    public MyTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Integer compute() {
        if (end - start <= THRESHOLD) {
            // Perform the task directly if it's below the threshold
            int sum = 0;
            for (int i = start; i < end; i++) {
                sum += array[i];
            }
            return sum;
        } else {
            // Divide the task into smaller subtasks
            int mid = (start + end) / 2;
            MyTask leftTask = new MyTask(array, start, mid);
            MyTask rightTask = new MyTask(array, mid, end);

            // Fork the subtasks for parallel execution
            leftTask.fork();
            int rightResult = rightTask.compute();
            int leftResult = leftTask.join();

            // Combine the results from subtasks
            return leftResult + rightResult;
        }
    }
}
```

## 2. Create a ForkJoinPool and invoke the task

- In your main program, create a `ForkJoinPool` instance and invoke the Fork/Join task using `invoke()` method

### Example Code

```Java
import java.util.concurrent.ForkJoinPool;

public class Main {
    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        // Create a ForkJoinPool with the default parallelism level
        ForkJoinPool forkJoinPool = new ForkJoinPool();

        // Create the Fork/Join task
        MyTask task = new MyTask(array, 0, array.length);

        // Invoke the task and get the result
        int result = forkJoinPool.invoke(task);

        // Print the result
        System.out.println("Result: " + result);
    }
}
```

## 3.  Adjust the Threshold and Task Logic

- Adjust the `THRESHOLD` value in your `RecursiveTask` subclass based on the size of your problem 
- Smaller tasks lead to more parallelism but increased overhead due to task creation and management
- Ensure the task logic in the `compute()` method is suitable for parallel execution. Tasks should be independent and able to run concurrently without interfering with each other. 

## 4. Handle [[Synchronisation]] (if necessary)
 
- if the task involves shared resources that require synchronisation, ensure proper synchronisation mechanisms (such as `synchronised` blocks or `java.util.concurrent` locks) are used to avoid data inconsistencies and [[race conditions]].

___
# Sequential Cut-off

>"sequential cutoff" refers to a threshold or a specific size of a problem beyond which it is more efficient to solve the problem sequentially rather than in parallel

$$TotalTime = O(n(P + logn))$$
___
# When Fork/Join is useful

• When you are doing the parallel computation many times
• When threads have a lot to do
• When threads have different amounts of work to do – load imbalance
	• Though unlikely for sum, in general sub problems may take significantly different amounts of time
	• Example: Apply method f to every array element, but maybe f is much slower for some data items
	• Example: Is a large integer prime?