- `RecursiveAction` is another generic class in the `java.util.concurrent` package that extends `ForkJoinTask` and represents a task that does not return a result.

- When you extend `RecursiveAction`, you provide an implementation for the `compute()` method, which performs the task without returning any value.

- Example usage: If your Fork/Join task performs an action but does not need to return a result, you extend `RecursiveAction`.

### Example Code

```Java
import java.util.concurrent.RecursiveAction;

public class MyRecursiveAction extends RecursiveAction {
    private int threshold;
    private int[] array;
    private int start;
    private int end;

    public MyRecursiveAction(int threshold, int[] array, int start, int end) {
        this.threshold = threshold;
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected void compute() {
        // Perform the task logic without returning a result
        // ...
    }
}
```

### Example HelloWorld code with RecursiveAction

```Java
import java.util.concurrent.ForkJoinPool; 
import java.util.concurrent.RecursiveAction;

public class HelloMany extends RecursiveAction {
	int greetings;
	int offset;

	HelloMany(int g, int start) {
		greetings = g;
		offset = start;
	}

	protected void compute() {
		if ((greetings) <= 1) {
			System.out.println("hello" + offset);
		} else {
			int split = (int) (greetings/2.0);
			HelloMany left = new HelloMany(split, offset);
			HelloMany right = new HelloMany(greetings-split,offset+split);
			left.fork();
			//left.join() // If this is here, parallelism is lost
			//right.join() // If this is here, right wont compute
			right.compute();
			left.join()
		}
	}

	public static void main(String[] args) {
		HelloMany sayHello = new HelloMany(50, 0); // Task to be done
		ForkJoinPool pool = new ForkJoinPool(); // Pool of worker threads
		pool.invoke(sayHello); // Give task to pool of threads
		//
	}
}
```

- `compute()` - a method from the `RecursiveAction` class that is overridden.
	- it is called implicitly when a task is invoked to the pool of [[threads]]
- `ForkJoinPool pool = new ForkJoinPool()` - Create a pool of worker threads
- `invoke(task)` - Invokes the task, waits for completion, and returns a result