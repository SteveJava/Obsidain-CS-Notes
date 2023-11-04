
- `RecursiveTask` is a generic class in the `java.util.concurrent` package that extends `ForkJoinTask` and represents a task that returns a result.
- When you extend `RecursiveTask<T>`, you need to provide an implementation for the `compute()` method, which performs the task and returns a result of type `T`.
- Example usage: If you want a [[Fork join]] task to return a result after its execution, you extend `RecursiveTask<T>`.

### Example code
```Java
import java.util.concurrent.RecursiveTask;

public class MyRecursiveTask extends RecursiveTask<Integer> {
    private int threshold;
    private int[] array;
    private int start;
    private int end;

    public MyRecursiveTask(int threshold, int[] array, int start, int end) {
        this.threshold = threshold;
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Integer compute() {
        // Perform the task logic and return the result
        // ...
    }
}
```

