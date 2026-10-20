---
share: true
---
1. Limitations of `synchronized` Blocks
- **No Manual Control**: Automatically acquires and releases locks without programmatic control over lock timing or scope.
- **No Non-Blocking Acquisition**: If a thread cannot acquire a lock, it immediately enters a blocked/waiting state. You cannot specify alternative (`else`) logic if the lock is unavailable.
- **No Lock Timeouts**: Threads cannot attempt to acquire a lock for a specified time before giving up.
- **Lack of Fairness**: `synchronized` does not grant locks in FIFO order. Threads arriving later can acquire the lock before earlier threads, leading to **thread starvation**.

2. The `java.util.concurrent.locks.Lock` Interface
- Acts as an explicit contract in Java's concurrency package providing greater flexibility than `synchronized`.
- **Core Methods**: `lock()` to acquire permission, and `unlock()` to release it.
- **Idiomatic Usage**: Always call `lock()` outside a `try` block, and invoke `unlock()` inside a **finally** **block** so the lock is released even if exceptions occur.

3. `ReentrantLock`
- **Reentrancy Feature**: A thread can acquire the same lock multiple times without deadlocking itself. It tracks the **owning thread** and a **hold count**, requiring an equal number of `unlock()` calls to fully release the lock.
- **Key Capabilities**:
    - `tryLock()`: Non-blocking attempt to acquire the lock immediately. Returns `true` if acquired, or `false` if unavailable, allowing alternative execution paths.
    - `tryLock(long timeout, TimeUnit unit)`: Waits up to the specified duration for the lock before returning `false`.
    - `isLocked()`, `isHeldByCurrentThread()`, `getHoldCount()`: Utility methods for monitoring and debugging lock status.
- **Fair vs. Unfair Locking**:
    - **Unfair Lock (Default)**: `new ReentrantLock()` does not enforce ordering, offering higher throughput but risking starvation.
    - **Fair Lock**: `new ReentrantLock(true)` grants access based on approximate waiting time (FIFO order) to prevent starvation.

4. `ReadWriteLock` & `ReentrantReadWriteLock`
- Designed for the **Reader-Writer Problem** where reading data is non-destructive (safe) and writing is destructive (requires mutual exclusion).
- Splits access into two lock types:
    - **Read Lock (Shared)**: Allows multiple reader threads to access the resource concurrently as long as no writer holds the lock.
    - **Write Lock (Exclusive)**: Allows only one writer thread at a time; blocks all readers and other writers.
- **Writer Starvation**: A continuous stream of incoming readers holding the shared lock can indefinitely delay waiting writers.
- **Lock Downgrading**: A thread holding a Write Lock can acquire a Read Lock and then release the Write Lock. **Lock Upgrading** (Read $\rightarrow$ Write) is **not allowed** as it causes deadlocks.

5. `StampedLock`
- Introduced in Java 8 as a faster, modern alternative to `ReadWriteLock`.
- Uses a `long` **stamp** (a ticket/token ID) returned upon lock acquisition and passed back during release.
- **Not Reentrant**: Acquiring the same `StampedLock` twice on the same thread results in a deadlock.
- **Three Modes**:
    1. **Write Lock**: Exclusive pessimistic lock.
    2. **Read Lock**: Shared pessimistic lock.
    3. **Optimistic Read (****tryOptimisticRead()****)**:
        - Returns a stamp without acquiring an actual lock.
        - Reads the data, then validates the stamp via `lock.validate(stamp)`.
        - If `validate()` returns `true`, no write occurred during the read. If `false`, falls back to a standard pessimistic Read Lock.

6. `Semaphore`
- Manages access to shared resources using a fixed number of **permits**.
- **Core Methods**:
    - `acquire()`: Takes a permit (decrements count). If no permits remain, the calling thread blocks.
    - `release()`: Returns a permit (increments count).
- **Types**:
    - **Binary Semaphore** (`permits = 1`): Functions similarly to a mutex lock.
    - **Counting Semaphore** (`permits > 1`): Limits concurrent execution to $N$ threads.
- **No Thread Ownership**: Unlike explicit locks, **any thread can call** **release()**, regardless of which thread called `acquire()`.
- **Common Use Cases**: API rate limiters, database connection pooling, or bounding parallel task execution.

7. `Condition` Interface
- Modern alternative to `Object.wait()`, `notify()`, and `notifyAll()` bound to explicit `Lock` objects (`lock.newCondition()`).
- **Method Equivalence**:
    - `await()` $\rightarrow$ `wait()`
    - `signal()` $\rightarrow$ `signalAll()` / `notify()`
    - `signalAll()` $\rightarrow$ `notifyAll()`
- **Key Advantage (Multiple Wait-Sets)**: Allows creating multiple distinct `Condition` instances on a single lock (e.g., `notFull` and `notEmpty` queues in a Producer-Consumer buffer). This targets wake-ups precisely to specific thread types and eliminates cross-wakeup issues.
- Must always be invoked within a `while` loop (Guarded Block) to protect against **spurious wakeups**.

### **1. `ReentrantLock`**

```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantLockDemo {
    private final Lock lock = new ReentrantLock();

    public void performTask() {
        lock.lock(); // Acquire lock
        try {
            System.out.println(Thread.currentThread().getName() + " acquired lock.");
        } finally {
            lock.unlock(); // Always release lock in finally block
        }
    }

    public void tryPerformTask() {
        if (lock.tryLock()) { // Non-blocking lock attempt
            try {
                System.out.println("Lock acquired via tryLock().");
            } finally {
                lock.unlock();
            }
        } else {
            System.out.println("Lock busy, executing alternative path.");
        }
    }
}

```

---

### **2. `ReentrantReadWriteLock`**

```
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReadWriteLockDemo {
    private final ReadWriteLock rwLock = new ReentrantReadWriteLock();
    private int sharedData = 0;

    // Shared lock allowing concurrent readers
    public int readData() {
        rwLock.readLock().lock();
        try {
            return sharedData;
        } finally {
            rwLock.readLock().unlock();
        }
    }

    // Exclusive lock allowing only one writer at a time
    public void writeData(int value) {
        rwLock.writeLock().lock();
        try {
            this.sharedData = value;
        } finally {
            rwLock.writeLock().unlock();
        }
    }
}

```

---

### **3. `StampedLock`**

```
import java.util.concurrent.locks.StampedLock;

public class StampedLockDemo {
    private final StampedLock lock = new StampedLock();
    private int data = 42;

    // Optimistic reading without acquiring a pessimistic lock
    public int readDataOptimistic() {
        long stamp = lock.tryOptimisticRead();
        int currentData = this.data;

        // Fall back to pessimistic read lock if data was modified during read
        if (!lock.validate(stamp)) {
            stamp = lock.readLock();
            try {
                currentData = this.data;
            } finally {
                lock.unlockRead(stamp);
            }
        }
        return currentData;
    }

    // Exclusive write lock
    public void writeData(int newData) {
        long stamp = lock.writeLock();
        try {
            this.data = newData;
        } finally {
            lock.unlockWrite(stamp);
        }
    }
}

```

---

### **4. `Semaphore`**

```
import java.util.concurrent.Semaphore;

public class SemaphoreDemo {
    // Limits concurrent access to a maximum of 3 threads
    private final Semaphore semaphore = new Semaphore(3);

    public void accessResource() {
        try {
            semaphore.acquire(); // Decrements available permit count
            System.out.println(Thread.currentThread().getName() + " acquired permit.");
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            semaphore.release(); // Increments available permit count
        }
    }
}

```

---

### **5. `Condition`**

```
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class BoundedBuffer {
    private final Lock lock = new ReentrantLock();
    private final Condition notFull  = lock.newCondition(); // Producer wait-queue
    private final Condition notEmpty = lock.newCondition(); // Consumer wait-queue

    private final Object[] items = new Object[10];
    private int count, putptr, takeptr;

    public void put(Object x) throws InterruptedException {
        lock.lock();
        try {
            while (count == items.length) {
                notFull.await(); // Producer waits until space opens up
            }
            items[putptr] = x;
            if (++putptr == items.length) putptr = 0;
            ++count;

            notEmpty.signal(); // Signal consumer that an item is available
        } finally {
            lock.unlock();
        }
    }

    public Object take() throws InterruptedException {
        lock.lock();
        try {
            while (count == 0) {
                notEmpty.await(); // Consumer waits until item is produced
            }
            Object x = items[takeptr];
            if (++takeptr == items.length) takeptr = 0;
            --count;

            notFull.signal(); // Signal producer that buffer has space
            return x;
        } finally {
            lock.unlock();
        }
    }
}
```