- [[Parallel Algorithms]] must be both **correct** and **efficient**
- To show **correctness**: validate an algorithm against a serial version to show it produces same result across a range of input
- To show **efficiency**: Benchmark an algorithm against a serial version to show it is faster

## ==Speedup==

>Speedup measures how much faster a parallel program runs compared to its sequential counterpart.

- If the sequential execution time (in seconds) is $$T_s$$​ and the parallel execution time (in seconds) is $$T_p$$​then the speedup (S) is calculated as follows:
$$Speedup = \frac{T_s}{T_p}$$
- ==**S>1==:** Indicates the ==parallel version== of the program is ==faster== than the sequential version. A speedup of 2, for example, means the parallel version is twice as fast as the sequential version.
    
- ==**S=1==:** Represents ==no improvement==; the parallel version performs at the same speed as the sequential version.
    
- **==S<1==:** Indicates the ==parallel version is slower== than the sequential version, which can occur due to parallelisation overhead, inefficient algorithms, or contention for resources.

- an algorithm is ==scalable== if the speedup increases at least linearly with the problem size.
- running time is inversely proportional to the number of processors used.

- Ideally, in a perfectly parallelizable task, the speedup would be linear, meaning the speedup (SS) equals the number of processors (pp) used. 

- This is known as ==**linear speedup**==. However, achieving linear speedup is not always possible due to factors such as parallelisation overhead, communication costs, and load balancing issues.

## Work and Span

In the context of parallel algorithms, **work** and **span** are two important metrics used to analyse the performance and efficiency of parallel computations.

### Work:

**Work** refers to the ==total amount of computation done by an algorithm to solve a problem==. It represents the total number of operations (or steps) performed by all processors in a parallel algorithm. Work is often denoted as $$T_1$$ and measures the time complexity of the best sequential algorithm for the given problem.

In the context of [[parallel algorithms]], the work is divided among multiple processors, and the goal is to reduce the total work compared to the sequential version of the algorithm. A parallel algorithm is considered efficient if it reduces the overall work while solving the problem.

### Span:

**Span** (also known as **critical path length** or **make-span**) represents ==the longest sequence of dependent computations in a parallel algorithm==. It measures the total time taken by the algorithm in the presence of parallelism, assuming an infinite number of processors. Span is often denoted as $$T_\infty$$
Span provides an upper bound on the execution time of the parallel algorithm and helps in understanding the parallelism available in the algorithm. The goal is to ==minimise the span== to reduce the overall execution time in a parallel computation.

### Relationship between Work and Span:

- **Work:** Represents the total amount of computation done by all processors in a parallel algorithm.
  
- **Span:** Represents the longest sequence of dependent computations in the algorithm.

For an ideal parallel algorithm with perfect parallelism (no dependencies, infinite processors), the execution time is ideally equal to the span. In real-world scenarios, parallel algorithms might have dependencies and communication overhead, leading to some deviation from this ideal relationship.

Efficient parallel algorithms balance the trade-off between work and span. Minimising work reduces the total computation, while minimising span reduces the overall execution time by utilising parallelism effectively. Achieving a good balance between work and span is essential for designing efficient parallel algorithms.
$$Max Speedup = \frac{Work}{Span} = \frac{T_1}{T_\infty} 
$$
## The DAG

- Program execution can be seen as a DAG (Directed Acyclic Graph)
- Nodes: Pieces of work  
- Edges: Source must finish before destination starts
- A fork “ends a node” and makes two outgoing edges
- A join “ends a node” and makes a node with two incoming edges
- work: number of nodes, T1
- span: length of the longest path, T∞ 
- Work is O(n)  
- Span is O(logn)  
- Maximum speedup is n/logn (grows exponentially)

## Performance with Java's [[Fork join]] Framework

We have Work Law:
$$
T_P>=T_1 /P
$$

We have Span Law:

$$
T_P >= T_\infty
$$


The fork-join framework guarantees expected time
$$T_P =O(T_1 /P)+O(T_\infty)$$

- No implementation of your algorithm can beat O(T∞) by more than a constant factor

- No implementation of your algorithm on P processors can beat (T1 / P) (ignoring memory-hierarchy issues)

## Amdahl's Law

>Amdahl’s Law is a mathematical theorem demonstrating the diminishing returns of adding more processors.

$$Speedup = \frac{1}{1-p + \frac{p}{n}}$$
Example

• Ten processors  
• 60% concurrent, 40% sequential • How close to 10-fold speedup?

$$Speedup = \frac{1}{1-0.6 + \frac{0.6}{10}}$$
$$Speedup = 2.17$$
### Two kinds of scalability

- strong scaling:
	- Fixed problem size
	- defined as how the solution time varies with the number of processors for a fixed total problem size.
- weak scaling:  
	- Fixed execution time
	- defined as how the solution time varies with the number of processors for a fixed problem size per processor.
	- Double the task when you double processors

$$Efficiency=T_1/T_P$$

$$Scaled Speedup= (1-p) + p × n$$

![[StrongSalingWeakScaling.png]]

