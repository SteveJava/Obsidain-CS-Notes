> An Executor is a basic support for **launching new tasks**

In large-scale parallel applications, it makes sense to **separate thread creation** and **management** from the rest of the application

**Executor objects** in java *distribute tasks* to worker [[threads]] in a **thread pool**

> **ExecutorService**, sub-interface of Executor, adds features to **manage tasks.**

There are three executor interfaces in java. We focus on [[Fork join]].
