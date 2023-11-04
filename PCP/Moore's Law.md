## Using Multiple Cores

- [[Parallel programming]] is **essential** to use the computing power of **multi-core architectures**

### The Era of Multicore

- We cannot keep making "wires exponentially smaller", so put **multiple processors** on the same chip (multicore)

### The Death of Moore's Law

> Clock speeds for desktop computers are doubled every 18-24 months

- Eventually reached the **power wall**:

	- Increasing clock rate generates **too much heat**: power and cooling constraints limit increases in microprocessor clock speeds
	- Relative cost of memory is too high

### Clarification

- *Real concurrent execution*: If the computer has multiple processors, then instructions from a number of processes/threads, equal to the number of physical processors, can be executed at the **same time**.

- *Time-slicing:* it is usual to have **more active processes** then **processors**. The available processes are switched between processors. 
