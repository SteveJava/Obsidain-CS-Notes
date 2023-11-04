> **Synchronisation constraints** are requirements pertaining to the order of the events

- This is the programmers' responsibility to ensure that synchronisation constraints are enforced
- "the Complier" has no idea what inter-leavings should or shouldn't be allowed to do in your program
- Example:
	- **Signalling**: Event A must happen *before* Event B

## The Mutual Exclusion Problem

> Several processes compete for a resource, but the nature of the resource requires that only one process access the resource at a time

- If two processes try to enter a **critical section**, one process must win and the other must **block:**
	- Not proceed until the "winning" process has completed

### Where Mutual Exclusion is necessary

1. Processing different bank account operations
	- What if 2 [[Threads]] change the same account at the same time?
2. Using a shared cache
3. Creating a pipeline

### Solving Mutual Exclusion

- `join()` is not what we want
- We need to block until another thread is "done using what we need" not "completely done executing"

### We need Atomic Actions

- Operations A and B are atomic with respect to each other if, from the perspective of a thread executing A, when another thread executes B, either all of B has executed or non of it has
- The operation is *indivisible*

## Atomic Classes

- `java.util.concurrent` contains **atomic variable classes** for atomic actions on numbers and objects
- `AtomicInteger`, `AtomicLong`, `AtomicBoolean`, `AtomicReference` all have `getAndSet()` methods
- For example, we can replace a Long counter variable with `AtomicLong` to ensure that all updates to the counter state are **atomic**

### Example code

```Java
public class Counter {

private AtomicLong value; // this is a class! Counter() { value=new AtomicLong(0); }  
public long get() { return value.get(); } public void set(long val) { value.set(val); } public void incr() { value.getAndIncrement(); }

}
```

### Limitations of Atomic Variables

- Atomic variables make a class thread-safe only if ONE variable defines the class **state** and there are no **compound actions** accessing this **state**.
- To preserve **state consistency**, you should update related state variable in a **single** atomic operation

### Example code: Atomic won't work

```Java
class BankAccount {
	private int balance = 0;  
	int getBalance(){ return balance; } 
	void setBalance(int x) { balance = x; }

	// withdraw is a compound action
	void withdraw(int amount) { 
		int b = getBalance(); 
		if(amount > b)
			throw new WithdrawTooLargeException(); 
		setBalance(b – amount);
	}
 // other operations like deposit, etc. 
}
```

## Interleaving

Suppose:
- Thread `T1` calls `x.withdraw(100)`
- Thread `T2` calls `y.withdraw(100)`

If calls **interleave**
- and `x` and `y` refer to different accounts:
	- no data races
- and `x` and `y` refer to aliases:
	- possible data race

## Synchronised Methods

- A built-in locking mechanism in Java for enforcing **atomicity**
- A method/function can be made **mutually exclusive** by prefixing the method with the keyword **synchronised**

`synchronized void f() { body; }`

### Example code

```Java
class BankAccount {

	private int balance = 0; 
	synchronized int getBalance() 
		{ return balance; }  
	synchronized void setBalance(int x)
		{ balance = x; }  
	synchronized void withdraw(int amount) {
         int b = getBalance();
         if(amount > b)
           throw ...
         setBalance(b – amount);
	}
// deposit would also use synchronized
}
```

### Synchronised block

- **Critical sections** of code can be made **mutually exclusive** by prefixing the method with the keyword **synchronised**
	- `synchronized void f() { body: }` is equivalent to:
	- `void f() { synchronized(this) { body; } }`
- a **synchronised block** has two parts:
	- a reference to an object that will serve as the `lock`
	- a block of code to be guarded by that `lock`
		`synchronized (object) { stetements }`

## Locks

- Every object (but not primitive types) "is a lock" in Java

 - A lock is an Abstract Data Type with operations:
	 - **new:** make a new lock, initially *not held*
	 - **acquire**: blocks if this lock is already currently *held*
	 - **release**: makes this lock *not held*
		 - if >=1 threads are blocked on it, exactly 1 will acquire it

- The lock implementation ensures that upon simultaneous acquires and/or releases, a correct thing will happen
	- Example: Two acquires: one will "win" and one will block
- How can this be implemented?
	- Need to "check and update" "all-at-once"
	- Uses special hardware and O/S support

### Example code

```Java
class BankAccount {

      private int balance = 0;
      private Lock lk = new Lock();
//...  
	void withdraw(int amount) {
		lk.acquire(); /* may block */
        int b = getBalance();
        if(amount > b)
			throw new WithdrawTooLargeException(); 
		setBalance(b – amount);  
		lk.release();
	}
	// deposit would also acquire/release lk 
}
```

### Problems with locks

- A lock is a very **primitive mechanism**
	- still up to the programmer to implement critical sections
- **Incorrect**: Use different locks for different methods
	- Mutual exclusion works only when using **same lock**
- Poor performance: Use same lock for every object
	- No simultaneous operations on different objects
- Incorrect: Forget to release a lock (blocks other threads forever! )

### Locks have to be re-entrant

> A Re-entrant lock "remembers" the thread (if any) that holds it, via use of a `count`

- When the lock goes from *not-held* to *held*, the count is 1
- If (code running in) the current holder calls **acquire**:
	- it does not block
	- it increments the count
- On `release`:
	- if the count > 0, the count is *decremented*
	- if the count is 0, the lock becomes *not-held*

### Java does it for you

- Every object created, including every class loaded, has an associated **lock** (or **monitor**)
- Putting code inside a synchronised block makes the compiler append instructions to **acquire** the lock on the specified object before executing the code, and **release** it afterwards
- Between acquiring the lock and releasing it, a **thread** is said to "own" the lock 
- **At the point of Thread A** wanting to **acquire the lock, if Thread B** already **owns it**, then **Thread A must wait** for **Thread B to release it**

### Locks Summary

Every Java object possesses one lock  
• Manipulated only via synchronised keyword
• Class objects contain a lock used to protect statics  
• Scalars like int are not Objects, so can only be locked via their enclosing objects

Java locks are reentrant
• A thread hitting synchronised passes if the lock is free or it already possesses the lock, else waits
• Released after passing as many }’s as {’s for the lock • cannot forget to release lock

### Block synchronisation vs method synchronisation

- **Block synchronisation** is preferred over **method synchronisation**:
	- with block synchronisation you only need to lock the critical section of code, instead of whole method.
- Synchronisation comes with a performance cost:
	- we should synchronise only part of code which absolutely needs to be synchronised
