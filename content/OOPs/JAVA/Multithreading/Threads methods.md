---
share: true
---
### **Quick Reference Summary**

| Method                        | Scope    | Target State                | Throws `InterruptedException`? | Key Architectural Behavior                                                      |
| ----------------------------- | -------- | --------------------------- | ------------------------------ | ------------------------------------------------------------------------------- |
| **Thread.sleep(ms)**          | Static   | `TIMED_WAITING`             | Yes                            | Pauses current thread; **retains acquired locks**.                              |
| **join()** **/** **join(ms)** | Instance | `WAITING` / `TIMED_WAITING` | Yes                            | Calling thread waits until target thread terminates (or timeout expires).       |
| **Thread.yield()**            | Static   | `RUNNABLE`                  | No                             | Cooperative hint to give CPU quantum to equal-priority threads; OS may ignore[. |
| **interrupt()**               | Instance | Unchanged                   | No (triggers it in target)     | Sets interrupt status flag to `true` for graceful cancellation                  |
| **isAlive()**                 | Instance | N/A                         | No                             | Returns `true` between `start()` and termination.                               |
| **setDaemon(true)**           | Instance | N/A                         | No                             | Marks thread as background daemon; must be set prior to `start()`.              |

### **1. `Thread.sleep(long millis)`**
- **Function:** Suspends the execution of the currently executing thread for a specified duration in milliseconds
- **State Transition:** Moves the thread from `RUNNABLE` to `TIMED_WAITING`, returning to `RUNNABLE` once the time elapses
- **Exception:** Throws `InterruptedException`, a checked exception requiring explicit `try-catch` handling or declaration via `throws`
- **Lock Retention:** Calling `sleep()` pauses thread execution **without releasing any acquired monitor locks**, forcing competing threads waiting on those locks to remain blocked
### **2. `join()` & `join(long millis)`**
- **Function:** Forces the calling thread (e.g., `main`) to pause execution until the target thread finishes its task.
- **Parameterless** **join()** **:** Moves the caller to the `WAITING` state until the target thread reaches `TERMINATED`.
- **Timed** **join(millis)** **:** Overloaded version where the caller waits only for the specified duration (`TIMED_WAITING`); if the target thread does not finish within that period, the caller resumes anyway.
- **Determinism:** Solves the non-deterministic interleaving of threads by enforcing explicit sequencing dependencies between tasks.
- **Exception:** Throws `InterruptedException` if interrupted while waiting.
### **3. `Thread.yield()`**
- **Function:** A static method indicating that the current thread is willing to surrender its current CPU time slice to other ready threads of equal priority.
- **State Transition:** Moves the thread to `RUNNABLE`; it **never** transitions to `WAITING`, `TIMED_WAITING`, or `BLOCKED`.
- **OS Scheduler Dependency:** Treated merely as a non-binding suggestion/request to the OS scheduler. The OS can freely ignore it, making its behavior unpredictable and largely unsuitable for production code
### **4. Thread Interruption (`interrupt`, `isInterrupted`, `interrupted`)**
Java does not support forcibly stopping threads, relying instead on a cooperative signaling mechanism centered around an internal boolean flag (`interrupt flag`, default: `false`).
- **t.interrupt()** **:** Sets the target thread's interrupt flag to `true`. It does not abruptly stop code execution.
- **Graceful Thread Cancellation:** A worker thread checks its own status (e.g., inside a loop condition `!Thread.currentThread().isInterrupted()`) and cleanly exits when flagged.
- **isInterrupted()** **vs.** **Thread.interrupted()** **:**
    - **t.isInterrupted()** **(Instance):** Inspects and returns the flag's current boolean status without modifying it.
    - **Thread.interrupted()** **(Static):** Returns the current thread's interrupt status and **resets the flag back to** **false**.
- **Impact on Waiting/Sleeping Threads:** If a thread is in `WAITING` or `TIMED_WAITING` (via `sleep()` or `join()`) when `interrupt()` is called, it wakes up prematurely, clears its interrupt status flag, and throws `InterruptedException`.
### **5. `isAlive()`**
- **Function:** Returns `true` if the thread has been started via `.start()` and has not yet completed its `run()` method.
- **Lifecycle Boundary:** Returns `false` in the `NEW` state (before `start()`) and in the `TERMINATED` state (after execution finishes).
### **6. Identification & Inspection (`currentThread`, `setName`, `getName`)**
- **Thread.currentThread()** **:** Static method providing an object reference to the thread currently executing the instruction.
- **Naming:** Default naming follows patterns like `"main"`, `"Thread-0"`, etc.
- **setName(String)** **&** **getName()** **:** Assigns and inspects custom identifiers, which are vital for diagnostics, debugging, and tracing worker threads in logs and thread pools.
- **getId()** **Status:** Deprecated in modern Java because IDs do not correlate with thread execution priority or sequence.
### **7. Thread Priorities (`setPriority`, `getPriority`)**
Every Java thread possesses an integer priority ranging from 1 to 10 that provides a scheduling hint to the OS:

- **Thread.MIN_PRIORITY** = `1`
- **Thread.NORM_PRIORITY** = `5` (Default)
- **Thread.MAX_PRIORITY** = `10`
- **Enforcement:** Schedulers are not bound to honor Java priorities; the underlying OS may respect them, partially weigh them, or disregard them completely
### **8. User Threads vs. Daemon Threads**
Java divides all threads into two categories:
```
Java Threads
 ├── User Threads    ──► High priority / Primary application logic (e.g., main thread)
 └── Daemon Threads  ──► Low priority background service tasks (e.g., Garbage Collector)
```
- **User Threads:** Crucial tasks that keep the JVM runtime alive; the application process will not terminate as long as at least one user thread remains active, even if `main` has finished.
- **Daemon Threads:** Background service providers. As soon as all user threads finish executing, **the JVM halts immediately**, abruptly terminating all daemon threads regardless of their execution state.
- **Configuration:** Configured using `t.setDaemon(true)` strictly **before** invoking `t.start()`.
- **Primary Real-World Example:** The Java **Garbage Collector (GC)** runs as a daemon thread so it automatically shuts down once user application code ceases