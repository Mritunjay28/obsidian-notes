---
share: true
---

### **1. `Future` Overloaded `submit()` Methods & Core API**

- **Future** **Interface**: Represents the result of an asynchronous computation.
- **submit()** **Overloads on** **Executor**:
    - `submit(Callable<T>)`: Submits a value-returning task and returns a `Future<T>` 
    - `submit(Runnable)`: Submits a task with no return value and returns `Future<?>` (`get()` returns `null`), allowing status checks via `isDone()` or cancellation via `cancel()`.
    - - `submit(Runnable, T result)`: Submits a non-returning task alongside a predefined result object returned by `get()` upon completion.
- **get()** **vs.** **get(timeout, unit)**:
    - Standard `get()` blocks the calling thread indefinitely until the task completes.
    - Overloaded `get(timeout, unit)` waits up to the specified duration before throwing a `TimeoutException`.
    - `get()` throws checked exceptions: `InterruptedException` and `ExecutionException`.
- **Cancellation & Status Methods**:
    - `isDone()`: Non-blocking check returning `true` if the task is finished.
    - `cancel(mayInterruptIfRunning)`: Cancels the task. Unstarted tasks cancel immediately; actively running tasks attempt thread interruption if `true`, or run to completion if `false`.
    - `isCancelled()`: Returns `true` if the task was cancelled prior to completion.
- **Limitations of** **Future**:
    - **Blocking Nature**: Calling `get()` forces synchronous blocking, destroying asynchronous parallelism.
    - **Passive Nature**: Cannot easily chain downstream tasks or combine multiple asynchronous results.

---

### **2. `CompletableFuture`**

- **Overview**: Advanced non-blocking `Future` implementation widely used in production applications. Uses `ForkJoinPool.commonPool()` by default.
- **Static Constructors**:
    - `CompletableFuture.supplyAsync(Supplier<u>)`: Executes a value-returning task asynchronously.
    - `CompletableFuture.runAsync(Runnable)`: Executes a void-returning task asynchronously.
    - Both accept an optional custom `Executor` as a second argument.
- **Non-Blocking Task Chaining Methods**:
    - `thenApply(Function)`: Asynchronously transforms intermediate results and returns a new `CompletableFuture`.
    - `thenAccept(Consumer)`: Consumes the result (e.g., logging) without returning a value, producing `CompletableFuture<Void>` 
    - - `thenRun(Runnable)`: Executes a `Runnable` after the previous stage finishes, without receiving inputs or returning outputs.
    - `thenCombine(CompletableFuture, BiFunction)`: Combines asynchronous outputs from two independent futures once both complete.

---

### **3. `ForkJoinPool` & Work-Stealing Architecture**

- **Paradigm**: Implements the **Divide-and-Conquer** approach by recursively splitting large tasks into independent subtasks up to a base condition, running them in parallel, and combining the results.
- **Queue Structure**:
    - **Submission Queue (Shared)**: Global blocking queue where externally submitted tasks arrive.
    - **Work-Stealing Queues (Private Deques)**: Every worker thread maintains its own private task queue.
- **Work-Stealing Algorithm**:
    - A worker thread divides a task, executes one subtask, and pushes the remaining subtasks onto its private queue.
    - Idle worker threads check the global **Submission Queue** first; if empty, they "steal" pending subtasks from the tail of another busy thread's private queue.
- **Task Abstractions**:
    - `RecursiveTask<V>` : Extended when subtasks compute and return a result (`V`) via `compute()`.
    - `RecursiveAction`: Extended when subtasks execute without returning a value.
- **Core Execution Operations**:
    - `fork()`: Pushes a subtask onto the current thread's private work-stealing queue for asynchronous execution.
    - `join()`: Blocks until the target subtask completes and returns its result.

---

### **4. `ThreadLocal`**

- **Concept**: Confines variables to individual threads in heap memory, giving each thread an isolated copy.
- **Benefits**: Eliminates race conditions and synchronization lock overhead without limiting variable scope to short-lived function stack frames.
- **Core API**: `set(value)`, `get()`, and `remove()`.

---

### **5. Virtual Threads (Java 21+ / Project Loom)**

- **Platform Threads (Traditional)**:
    - 1-to-1 mapping with OS threads.
    - Heavyweight: Allocates 512 KB to 1 MB memory per thread, leading to high context-switching and creation overhead.
- **Virtual Threads**:
    - Managed entirely by the JVM rather than the OS.
    - Lightweight with minimal memory footprint, enabling hundreds of thousands of concurrent virtual threads.
    - **1-to-Many / M-to-N Mapping**: A small pool of OS carrier threads (managed via `ForkJoinPool`) executes many virtual threads.
- **Unmounting Mechanic on Blocking Operations**:
    - When a virtual thread encounters blocking I/O or lock waits, the JVM automatically **unmounts** it from its underlying OS carrier thread.
    - The OS carrier thread immediately switches to execute another runnable virtual thread, eliminating OS thread idle time.
- **Creation Methods**: `Thread.startVirtualThread(Runnable)` or `Executors.newVirtualThreadPerTaskExecutor()`.