### 1. Sequential Model

A running serial program has:
- one *program counter* (current statement executing)
- one *call stack* 
- *objects*
- *Static fields* of classes

### 2. [[Shared Memory]] Model

[[Threads]] are basically mini serial programs 

Each thread has its own: 
- program counter 
- call stack 
- local variables

All threads share one collection of objects and static fields
- Static fields of classes are also shared by threads
- Threads communicate through shared objects (implicit communication)

### 3. Message Passing Model
 
>Use explicit threads that each contain their own objects/data

- Communication is via explicitly **sending/receiving messages**, maintaining **copies** of the data (share nothing)
- Most common on HPC models

### 4. Map Reduce Model

> Map: extract something you care about from each record
> Reduce: aggregate, summarise, filter or transform

- Developed by Google

- Have **primitives** for thing like "apply function to every element of an array in parallel"
