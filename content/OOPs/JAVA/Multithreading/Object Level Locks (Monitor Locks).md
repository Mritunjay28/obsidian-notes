---
share: true
---
### **1. Motivation & Core Guarantees**
- **Purpose:** Restricts access to a critical section so that only one thread can execute it at any given time, preventing race conditions and data corruption on shared resources.
- **Four Core Guarantees:**
    1. **Atomicity:** Ensures the entire critical section executes without thread interference, treating multi-step non-atomic operations (such as `count++`) as indivisible.
    2. **Visibility:** Forces variable updates to flush to main RAM on lock release and forces reads directly from main RAM on lock acquisition, bypassing stale local CPU caches.
    3. **Ordering:** Prevents harmful instruction reordering across thread boundaries.
    4. **Scoped Serial Execution:** Serializes execution strictly within the synchronized block/method while allowing surrounding non-critical code to run concurrently or in parallel.
### **2. Lock Acquisition Mechanics & Thread States**
- **Lock Acquisition:** A thread must acquire the monitor lock before entering a synchronized method or block.
- **The** **BLOCKED** **State:** If another thread already holds the monitor lock, competing threads fail to acquire it and transition into the `BLOCKED` state.
- **Retention During Context Switches:** If a thread holding a lock is context-switched or paused (e.g., via `Thread.sleep()`), it **retains lock ownership**; competing threads remain `BLOCKED` and cannot enter.
- **Re-entering** **RUNNABLE** **:** When the lock owner exits the critical section, it releases the lock. A `BLOCKED` thread transitions back to `RUNNABLE` and acquires the lock when scheduled.
### **3. Object-Level Locks (Monitor Locks)**
- **Every Object Has an Intrinsic Lock:** In Java, every object allocated on the heap possesses an internal monitor lock managed directly by the JVM.
- **Locks Belong to Objects, Not Methods:** Declaring an instance method `synchronized` acquires the monitor lock of the calling **object instance**, not the method itself.
- **Single Object, Single Lock:**
    - All synchronized instance methods within a class share the same object monitor lock.
    - If Thread 1 executes a synchronized method `m1()` on object `c1`, Thread 2 cannot execute another synchronized method `m2()` on that same instance `c1` simultaneously
- **Multiple Objects, Independent Locks:** Separate object instances (`obj1` vs `obj2`) have distinct monitor locks, allowing threads to execute methods on different instances concurrently without blocking.
### **4. Synchronized Methods vs. Synchronized Blocks**
- **Synchronized Methods:** Serializes the entire method body, which adds performance overhead if non-critical code is included.
- **Synchronized Block Syntax:** `synchronized (objectReference) { ... }`.
- **Granular Locking:** Allows locking strictly the lines containing critical section operations (e.g., `synchronized(this) { count++; }`), reducing lock contention and leaving non-critical code concurrent.
### **5. Custom Lock Objects**
- **Decoupling Independent Operations:** Relying on `synchronized(this)` locks the entire object instance, preventing unrelated operations (e.g., `deposit()` and `withdraw()`) from executing concurrently.
- **Explicit Lock Target Objects:** Instantiating dedicated lock tokens (`private final Object lock1 = new Object();`) allows independent operations to be locked separately, permitting concurrent execution between distinct methods.
- **The** **new Object()** **Anti-Pattern:**
    - Writing `synchronized(new Object()) { ... }` instantiates a brand-new object on the heap for every thread invocation.
    - Because every thread acquires the lock of a different object instance, **no mutual exclusion occurs**, rendering synchronization completely useless.
### **6. Static Synchronization (Class-Level Locks)**
- **Class-Level Locking:** Declaring a `static` method as `synchronized` locks the **Class** **object** (`Counter.class`) rather than an individual instance.
- **Block Syntax Equivalent:** `synchronized (Counter.class) { ... }`.
- **Independence of Instance vs. Class Locks:**
    - An instance-level monitor lock (`this`) and a class-level monitor lock (`Class.class`) are distinct and independent.
    - Thread 1 executing a static synchronized method and Thread 2 executing an instance synchronized method on the same object run concurrently without blocking each other.
### **7. Internal Conceptual Mechanics & CAS**
- **Internal Lock Representation:** Conceptually, a JVM monitor lock maintains:
    1. `ownerThread`: Reference to the thread currently holding the lock (null when free).
    2. `isLocked`: Boolean flag (`false` by default).
    3. `waitingQueue`: Queue storing threads placed in the `BLOCKED` state.
- **Atomic Lock Acquisition:** Checking and setting the lock status (`isLocked`) is executed atomically at the CPU instruction level using **Compare-And-Swap (CAS)** algorithms to prevent race conditions on the lock mechanism itself.
### **8. Overheads & Limitations**
- **Performance Cost:** Acquiring/releasing locks and moving threads between active and `BLOCKED` states introduces context-switching overhead.
- **Overuse Risk:** Excessive synchronization slows application throughput, leading modern production applications to often favor explicit locks or optimistic locking mechanisms.

### **1. Synchronized Method (Fixing Race Conditions)**
Adding `synchronized` to an instance method acquires the **intrinsic monitor lock of the calling object (** **this** **)**, making multi-step operations atomic.

```
class Counter {
    private int count = 0;

    // Synchronized method guarantees mutual exclusion and atomicity
    public synchronized void increment() {
        count++; // Critical section [1, 4]
    }

    public int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        // Two threads accessing the same instance
        Thread t1 = new Thread(() -&gt; {
            for (int i = 0; i &lt; 10000; i++) counter.increment();
        });
        Thread t2 = new Thread(() -&gt; {
            for (int i = 0; i &lt; 10000; i++) counter.increment();
        });

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Final Count: " + counter.getCount()); 
        // Always 20,000 
    }
}

```
### **2. Synchronized Block (`this`)**

Synchronizing only the critical section instead of the entire method body avoids unnecessary performance overhead.

```
class Counter {
    private int count = 0;

    public void increment() {
        // Non-critical operations run concurrently here 

        synchronized (this) { // Acquire lock on current instance 
            count++; // Critical section [7]
        }

        // Non-critical operations run concurrently here
    }
}

```
### **3. Object-Level Lock Behavior (`m1()` and `m2()`)**

All synchronized instance methods in a class share the **same single object monitor lock**.
```
class Test {
    public synchronized void m1() {
        System.out.println("M1 entered by " + Thread.currentThread().getName());
        try { Thread.sleep(2000); } catch (Exception e) {}
        System.out.println("M1 exiting");
    }

    public synchronized void m2() {
        System.out.println("M2 entered by " + Thread.currentThread().getName());
        try { Thread.sleep(2000); } catch (Exception e) {}
        System.out.println("M2 exiting");
    }
}

// Behavior:
// Test obj = new Test();
// Thread 1 calls obj.m1() -> Acquires obj's lock
// Thread 2 calls obj.m2() -> BLOCKED until obj.m1() completes 
// If obj1 and obj2 are distinct instances, t1.obj1.m1() and t2.obj2.m2() run concurrently .

```
### **4. Custom Lock Objects (Decoupling Operations)**

Instantiating distinct `Object` instances allows unrelated critical sections to execute concurrently without blocking each other.
```
class Bank {
    // Dedicated lock tokens 
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();

    public void deposit() {
        synchronized (lock1) { // Locked via lock1 
            System.out.println("Deposit processing...");
            try { Thread.sleep(2000); } catch (Exception e) {}
        }
    }

    public void withdraw() {
        synchronized (lock2) { // Locked via lock2 
            System.out.println("Withdrawal processing...");
            try { Thread.sleep(2000); } catch (Exception e) {}
        }
    }
}
// Result: One thread can deposit while another thread withdraws concurrently


# ⚠️ Anti-Pattern Warning: synchronized(new Object())

public void badMethod() {
    synchronized (new Object()) { // WRONG! Creates a new object on every call 
        // NO mutual exclusion occurs because every thread gets its own lock
    }
}

```
### **5. Static Synchronization (Class-Level Locks)**

Static synchronized methods acquire the lock on the **Class** **object (** **Counter.class** **)**, which is completely independent of instance locks.

```
class Counter {
    private static int count = 0;

    // Option A: Method-level static sync
    public static synchronized void increment() {
        count++;
    }

    // Option B: Block-level static sync (Functionally Identical) 
    public static void incrementBlock() {
        synchronized (Counter.class) { // Lock the Class object 
            count++;
        }
    }
}
```