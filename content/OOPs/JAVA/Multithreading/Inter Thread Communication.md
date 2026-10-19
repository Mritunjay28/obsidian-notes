---
share: true
---
1. Core Problem & Limitations of `synchronized`
- Multiple threads executing concurrently can cause **thread interference** or **race conditions** when accessing shared resources.
- While `synchronized` provides mutual exclusion by locking critical sections, it only ensures that one thread enters at a time.
- `synchronized` alone cannot coordinate execution order or messaging between threads (e.g., preventing a thread from consuming before data is produced).

2. The Producer-Consumer Problem
- A classic multithreading scenario where a **Producer** thread places data into a shared container/box and a **Consumer** thread retrieves it.
- **Multithreading Failure Modes (Non-deterministic Execution)**:
    - **Consumer runs first**: Attempts to consume data when the buffer is empty, reading `null`.
    - **Producer runs consecutively**: Overwrites unconsumed data before the consumer can read it, leading to data loss.

3. Naive Solutions & Why They Fail
**Naive Approach 1: Busy Waiting (Spin-Locking)**
- Continuously checking a status flag inside an infinite `while` loop (e.g., `while(flag == true) { /* do nothing */ }`).
- Requires the `volatile` keyword on shared variables to avoid thread caching and visibility issues.
- **Failure**: Wastes CPU cycles by constantly spinning without performing useful work ("Busy Waiting").

**Naive Approach 2: Synchronized Busy Waiting (Causes Deadlock)**
- Adding `synchronized` to busy-waiting methods.
- If the consumer enters first when no data exists, it acquires the object lock and spins infinitely in the loop without releasing it.
- The producer can never acquire the lock to produce data, resulting in a **Deadlock**.

4. Inter-Thread Communication Methods
Inter-thread communication relies on three methods from the `Object` class: `wait()`, `notify()`, and `notifyAll()`.

**wait()** **Method**
1. ==🟠**Releases all held monitor locks** on the object.==
2. Moves the calling thread from the `RUNNABLE` state into the `WAITING` state.
3. The thread remains paused until another thread calls `notify()` or `notifyAll()` on the same object.
4. ==🟠**Must be called within a** **synchronized** **context**; calling it outside throws an `IllegalMonitorStateException` because there is no lock to release.==

**notify()** **Method**
1. Selects **one random thread** from the object's waiting queue.
2. Moves that thread from `WAITING` to `BLOCKED` state so it can re-compete for the object lock.
3. Must be called inside a `synchronized` block/method, otherwise `IllegalMonitorStateException` is thrown.

**notifyAll()** **Method**
1. Wakes up **all threads** currently in the object's waiting queue, moving them to the `BLOCKED` state.
2. ==🟠All awakened threads compete for the lock; once a thread acquires the lock, it resumes execution.==
---

5. Internal Mechanics of Object Locks (Monitor Locks)
- Every Java object possesses an **intrinsic lock (Monitor Lock)** containing:
    - **Owner Thread**: Information on which thread currently holds the lock.
    - **Blocked Queue**: Threads waiting to acquire the lock.
    - **Waiting Queue**: Threads paused via `wait()` waiting to be notified.
- When `wait()` is called, the current thread releases ownership, moves to the **Waiting Queue**, and clears the owner field.
- When `notify()` is called, a thread is moved from the **Waiting Queue** to the **Blocked Queue**.
- Upon re-acquiring the lock, execution resumes **from the exact line where** **wait()** **was called**.

6. `notify()` vs. `notifyAll()` (Deadlock Risks in Multi-Thread Environments)
- **Single Producer/Consumer**: `notify()` is sufficient because only one thread is waiting at any time.
- **Multiple Producers and Consumers**: Using `notify()` can cause a **Deadlock**:
    - Example: $c_1$ consumes data and calls `notify()`. Instead of waking producer $p_2$, `notify()` randomly wakes consumer c2.
    - $c_2$ wakes up, finds no data, and goes back to `wait()`.
    - Now all remaining threads are stuck in the `WAITING` state with no active thread to notify them.
- **Solution**: ==🟠Always prefer `notifyAll()` in multi-threaded producer-consumer setups to ensure at least one eligible thread wakes up and proceeds.==

7. Core Concepts & Interview FAQs

| Feature / Question                                                                                                    | Explanation                                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Why are** **wait()** **,** **notify()** **, and** **notifyAll()** **in** **Object** **instead of** **Thread** **?** | Locks and waiting queues belong to individual **objects**, not threads. Since threads acquire/release object-level locks, the methods reside in `Object`.                                                 |
| **Thread.sleep()** **vs.** **Object.wait()**                                                                          | `sleep()` puts a thread in `TIMED_WAITING` for a fixed time and **holds onto locks**. `wait()` puts a thread in `WAITING` until notified and **releases held monitor locks**.                             |
| **Spurious Wakeup**                                                                                                   | A rare phenomenon where a thread wakes up from `WAITING` without being notified, caused by OS/CPU optimizations.                                                                                          |
| **Guarded Block Pattern (**while loop)                                                                                | To prevent spurious wakeups, always enclose `wait()` inside a `while` loop (e.g., `while(condition) wait();`). If a thread wakes spuriously, the loop re-checks the condition and forces it back to sleep |
