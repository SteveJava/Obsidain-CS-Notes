- Thread pools reduce creation **overhead**
- allocating and de-allocating many [[threads]] requires significant memory management

### Creating A Pool
- `ForkJoinPool` is the main thread pool class
- You can create the pool using: 
	`static final ForkJoinPool pool = new ForkJoinPool();`

- `ForkJoinPool()` creates a ForkJoinPool with **parallelism** equal to `Runtime.availableProcessors()
- You can specify the **parallelism**

### Using the Default Pool

Default pool can be accessed by using the `commonPool()` such as:

`static final ForkJoinPool pool = new ForkJoinPool.commonPool();`

- `commonPool()` is **static**, always available ad appropriate for most applications
- Has **parallelism** equal to Runtime.availableProcessors() -1
