> Ideal Computation: a computation that can be divided into a number of completely separate tasks; each can be executed by a single processor

## Embarrassingly Parallel Algorithms

>"Embarrassingly parallel" is a term[^1] used in parallel computing to describe problems or tasks that can be divided into many smaller subproblems that can be solved independently and simultaneously. 

- These subproblems require little or no communication or coordination between processing units. In other words, the problem can be parallelised without any effort or complexity, making it incredibly easy to implement parallel solutions.

- Tasks that are embarrassingly parallel are highly parallelizable, and they scale well with the addition of more processing units, such as CPU cores or GPUs. 
- The term "embarrassingly parallel" suggests that the parallelisation of the problem is so straightforward and simple that it might be embarrassing not to parallelise it.

### Examples of embarrassingly parallel problems include:

1. **Independent Data Processing:** Tasks that involve processing independent data sets where the computation for each set does not depend on the results of other sets.

2. **Monte Carlo Simulations:** Many Monte Carlo simulations involve running numerous independent simulations, and each simulation can be executed in parallel without affecting others.

3. **Rendering and Image Processing:** Tasks like image filtering, rendering frames in a movie, or processing pixels in an image can be parallelised on a per-pixel basis.

4. **Large-Scale Data Analysis:** Operations such as mapping, filtering, and reducing large datasets can often be parallelised because each data element can be processed independently.

5. **Distributed Computing:** Problems where different nodes in a distributed system perform computations independently and exchange only the final results, such as in certain types of distributed database queries.

In summary, embarrassingly parallel problems are those that can be effortlessly and efficiently parallelised, leading to significant speedup when executed on parallel computing resources. These problems are ideal candidates for parallel processing because they can fully utilise the available computational power without the complexity of managing dependencies or intricate communication patterns between processing units.

___
## Divide-and-conquer Parallel Algorithms

- Divide problems into sub problems that are of the same form as the larger problem
- 
1. Divide instance of problem into two or more smaller instances
2. Solve smaller instances recursively
3. Obtain solution to original(larger) instance by combining these solutions

- Recursive subdivision continues until the grain size of the problem is small enough to be solved sequentially.

- It is straight-forward to convert an embarrassingly parallel algorithm into a divide-and-conquer parallel algorithm.

## Examples of Divide-and-conquer algorithms:

Map and reduction algorithms are both fundamental operations in parallel and distributed computing, but they serve different purposes and have distinct characteristics. Here's how you can differentiate between the two:

### ==Map Algorithm:==

1. **Purpose:**
   - **Mapping** involves applying a function to each element in a dataset independently, transforming the data in some way.
   - **Example:** If you have a list of numbers and you want to square each number, you would use a map operation to apply the squaring function to every element in the list independently.

2. **Independence:**
   - Each element in the input dataset is processed independently of the others.
   - Parallelism is achieved by dividing the dataset into smaller chunks and processing these chunks concurrently on multiple processors or threads.

3. **Output:**
   - A map operation produces a one-to-one mapping between the input and output elements. If you start with N input elements, you end up with N output elements after applying the mapping function to each input element.

4. **Example Parallelisation:**
   - In a parallel map operation, different processors or threads work on different parts of the input dataset, applying the mapping function independently to each element.

### ==Reduction Algorithm==:

1. **Purpose:**
   - **Reduction** involves aggregating or combining multiple elements in a dataset to produce a single result.
   - **Example:** If you have a list of numbers and you want to calculate the sum of all numbers in the list, you would use a reduction operation to add all elements together to obtain a single value.

2. **Dependence:**
   - Reduction operations require a dependency between elements; the result of processing one element affects the processing of subsequent elements.
   - Parallel reduction involves breaking down the dataset into smaller chunks, processing them independently (potentially using a map operation), and then combining the intermediate results to obtain the final reduction result.

3. **Output:**
   - A reduction operation produces a single output value or a reduced representation of the input dataset. For example, the sum, average, maximum, or minimum value of the dataset.

4. **Example Parallelisation:**
   - In a parallel reduction operation, the input dataset is divided into smaller parts, and each part is reduced independently. Then, the intermediate results are combined using a binary tree-like structure until a single result is obtained.

In summary, map algorithms transform each element in a dataset independently, whereas reduction algorithms combine multiple elements to produce a single result. While map operations maintain independence between elements, reduction operations require dependencies and aggregation of results, leading to a single output value. Parallelisation in map operations focuses on processing elements independently, while parallel reduction involves combining intermediate results in a structured manner to obtain the final result.

[^1]: This is a referenced text