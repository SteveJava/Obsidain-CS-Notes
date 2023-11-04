
h>**Shared memory**: memory that can be simultaneously accessed by multiple processes

- Processors are connected by an **interconnection network**
- Each processor can access all locations in memory via a **single virtual address space** 

 - It allows multiple processes or threads to share a region of memory that can be accessed by any process or thread involved. 
 - This shared memory region is typically used to exchange data between different processes or threads running concurrently on the same system.

Here's how shared memory works:

1. **Memory Sharing:**
   - Two or more processes or [[threads]] are given access to the same area of physical memory.
   - These processes or threads can read from and write to this shared memory region.

2. **Communication:**
   - Processes or threads can communicate with each other by reading and writing data into this shared memory space.
   - For example, one process might write data into the shared memory, and another process can read that data from the same memory location.

3. **Synchronisation:**
   - Proper synchronisation mechanisms, such as locks or semaphores, are required to control access to the shared memory region to avoid conflicts and ensure data consistency.
   - Synchronisation is crucial to prevent race conditions where multiple processes or threads attempt to modify shared data simultaneously, leading to unpredictable behaviour.

- Shared memory is advantageous for inter-process communication because it provides a fast and efficient way for processes to exchange data. 
- However, it also requires careful coordination and synchronisation to prevent issues such as data corruption or race conditions.

- In multithreading, shared memory is often used to allow threads within the same process to communicate and collaborate. 
- Threads can share variables and data structures stored in shared memory, enabling them to work together on a common task. Proper synchronisation mechanisms, such as locks, are crucial to ensure thread safety and prevent data inconsistencies when multiple threads access shared memory concurrently.

- It's important to note that shared memory is limited to processes or threads running on the same machine. 
- For communication between processes or threads running on different machines, other IPC mechanisms like message passing or networking protocols are typically used.

- **Advantages**: faster memory communication
- **Disadvantages**: All processors using shared memory must make sure they are not writing to the same memory location