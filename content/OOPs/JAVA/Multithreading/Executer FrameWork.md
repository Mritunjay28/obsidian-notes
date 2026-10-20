---
share: true
---

### **1. Problems with Manual Thread Creation (`new Thread()`)**

- **High Memory Overhead**: Each Java thread allocates a default stack space of **512 KB to 1 MB**. Creating thousands of threads manually leads to massive memory consumption (500 MB – 1 GB+).
- **OS Scheduling Overhead & Thrashing**: CPU cores are limited. Spawning excessive threads forces continuous context switching, causing the CPU to spend more time context switching than performing actual work (**Thrashing**).
- **Expensive Creation Time**: Invoking `new Thread().start()` triggers system calls to request OS-level thread allocation, incurring high latency.
- **One-Time Use Waste**: Manually created threads execute a single task and terminate, leading to continuous thread allocation and garbage collection overhead.

---

### **2. Executor Framework & Thread Pool Core Concept**

- **Decoupling Task from Execution**: The developer only defines **what task** to execute asynchronously, leaving **how** it is executed to the framework.
- **Worker Threads & Task Queue**: A **Thread Pool** maintains a set of pre-created **Worker Threads** and a **Task Queue**.
- **Thread Reuse**: Once a worker thread completes a task, it does not terminate; it returns to the pool to pick up subsequent tasks, keeping context switching and resource allocation controlled.

---

### **3. Framework Hierarchy**

- **Executor** **(Interface)**: Top-level functional interface containing a single method: `void execute(Runnable command)`.
- **ExecutorService** **(Interface)**: Extends `Executor`. Adds lifecycle management (`shutdown()`, `shutdownNow()`) and task submission methods (`submit()`, `invokeAll()`, `invokeAny()`).
- **ScheduledExecutorService** **(Interface)**: Extends `ExecutorService` to handle delayed or periodic task execution (`schedule()`, `scheduleAtFixedRate()`).
- **Implementations**:
    - `ThreadPoolExecutor`: Standard, highly configurable thread pool.
    - `ForkJoinPool`: Specialized work-stealing thread pool.
    - `ScheduledThreadPoolExecutor`: Implementation for scheduled/periodic tasks.
- **Executors** **(Utility Class)**: Factory class providing pre-configured thread pool instances.

---

### **4. Task Execution Mechanics & Key Methods**

```
#### Runnable vs Callable<V>

- Runnable: `run()` method accepts no arguments, returns `void`, and cannot throw checked exceptions.
- Callable<V> : `call()` method returns a result of type `V` and can throw checked exceptions
  
#### execute() vs submit()

- execute(Runnable task): Submits a `Runnable` for execution and returns `void`. Unhandled exceptions inside the `Runnable` are lost/uncaught in caller threads.
- submit(Callable<T> task): Submits a `Callable` and returns a Future<T> representing the pending result. Exceptions thrown during execution are captured and re-thrown when retrieving the result
  
#### Future<T> & Blocking Retrieval

- Future<T> - : Represents an asynchronous, pending result.
- future.get(): Blocks the calling thread until the asynchronous computation completes and returns the result.

#### Lifecycle Termination

- shutdown(): Stops accepting new tasks but allows already submitted and queued tasks to finish before terminating.
- shutdownNow(): Stops accepting new tasks, clears the waiting queue, and attempts to interrupt active worker threads immediately
```

### **5. `ThreadPoolExecutor` Internals (5 Core Parameters)**

When creating a custom `ThreadPoolExecutor`, five primary parameters are configured:

1. **corePoolSize**: The minimum number of worker threads kept alive in the pool, even when idle.
2. **maximumPoolSize**: The maximum number of worker threads allowed in the pool.
3. **keepAliveTime**: The duration that extra idle threads (above `corePoolSize`) wait before terminating.
4. **unit**: Time unit for `keepAliveTime` (e.g., `TimeUnit.SECONDS`).
5. **workQueue**: A `BlockingQueue<Runnable > 1. used to hold tasks awaiting execution.

---

### **6. Task Processing Workflow & Allocation Rules**

When a new task is submitted to `ThreadPoolExecutor`:

1. **Check** **corePoolSize**: If active threads < `corePoolSize`, spawn a new worker thread to execute the task immediately.
2. **Check** **workQueue**: If active threads == `corePoolSize`, place the task into the `workQueue`.
3. **Check** **maximumPoolSize**: If the `workQueue` is full and active threads < `maximumPoolSize`, spawn a new extra worker thread to execute the task.
4. **Trigger Rejection Policy**: If the `workQueue` is full and active threads == `maximumPoolSize`, execute the configured **Rejected Execution Policy**.

---

### **7. Pre-Configured Pools via `Executors` Factory**

- **Executors.newFixedThreadPool(n)**: Fixed number of worker threads (`corePoolSize` = `maximumPoolSize` = `n`) using an unbounded `LinkedBlockingQueue`. Risk of `OutOfMemoryError` if tasks accumulate indefinitely.
- **Executors.newCachedThreadPool()**: `corePoolSize` = 0, `maximumPoolSize` = `Integer.MAX_VALUE`, 60-second keep-alive time. Spawns new threads on demand; can exhaust system resources under heavy traffic bursts.
- **Executors.newSingleThreadExecutor()**: Single worker thread (`corePoolSize` = `maximumPoolSize` = 1) with an unbounded queue. Guarantees sequential, serialized execution of tasks.
- **Executors.newScheduledThreadPool(n)**: Supports delayed execution (`schedule()`) and fixed-rate/fixed-delay periodic execution (`scheduleAtFixedRate()`).

---

### **8. Rejected Execution Policies**

When both worker threads and the queue are completely saturated:

- **AbortPolicy** **(Default)**: Rejects the task and throws a `RejectedExecutionException`.
- **DiscardPolicy**: Silently drops the rejected task without throwing an exception.
- **DiscardOldestPolicy**: Discards the unhandled oldest task at the head of the queue and retries submitting the new task.
- **CallerRunsPolicy**: Runs the rejected task in the thread that called `execute()` / `submit()`, throttling task submission speed.