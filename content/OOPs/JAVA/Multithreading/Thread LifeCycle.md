---
share: true
---
### **Approaches to Creating Threads in Java**

1. **Extending the `Thread` Class**
- **Mechanism:** Create a custom class that inherits from `java.lang.Thread` and override its `public void run()` method.
- **Execution Logic:** The `run()` method contains the code and task that the thread should execute.
- **Instantiation & Starting:** Instantiate the custom class and call its `.start()` method.

```
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start(); // Initiates new thread execution
    }
}
```

2. **Implementing the `Runnable` Interface**
- **Mechanism:** Create a class that implements the `java.lang.Runnable` interface and provides an implementation for the abstract `public void run()` method.
- **Underlying Architecture:** The built-in `Thread` class in Java itself implements the `Runnable` interface internally.
- **Instantiation & Starting:** Instantiate your `Runnable` implementation, pass that instance into the constructor of a `Thread` object (`new Thread(task)`), and call `.start()` on the thread object.

```
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Thread is running");
    }
}

public class Main {
    public static void main(String[] args) {
        MyRunnable r1 = new MyRunnable();
        Thread t1 = new Thread(r1);
        t1.start();
    }
}
```

3. **Using Lambda Expressions**
- **Functional Interface:** Because `Runnable` contains only one abstract method (`run()`), it qualifies as a **functional interface**.
- **Concise Syntax:** Rather than declaring a separate class or using anonymous inner classes, developers can pass an inline lambda expression directly to the `Thread` constructor.

```
Thread t1 = new Thread(() -&gt; {
    System.out.println("Thread is running via Lambda");
});
t1.start();
```

### **2. Extending `Thread` vs. Implementing `Runnable`**
Industry best practice consistently favors implementing `Runnable` over extending `Thread` for several architectural reasons:

| Architectural Criteria      | Extending `Thread`                                                                              | Implementing `Runnable`                                                                                          |
| --------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Core Concept**            | Defines a new **Thread** (IS-A relationship)                                                    | Defines a executable **Task** (WHAT to do)                                                                       |
| **Inheritance Flexibility** | Consumes the single class inheritance allowed in Java; the class cannot extend any other class. | Leaves the class free to extend any base class while implementing multiple interfaces                            |
| **Separation of Concerns**  | Blends thread lifecycle management with business task logic                                     | Cleanly separates the task definition (`Runnable`) from the execution mechanism (`Thread`)                       |
| **Code Reusability**        | Tight coupling makes sharing the logic across different execution units difficult               | High reusability; the same `Runnable` task instance can be assigned to multiple threads (e.g., `T1`, `T2`, `T3`) |
| **Modern Java Support**     | Cannot use lambda expressions directly.                                                         | Enables concise inline expressions via functional interfaces and lambdas.                                        |

### **3. Internal Mechanics: `start()` vs. `run()`**
Understanding the boundary between JVM instructions and Operating System scheduling is critical for multithreaded programming.

```
t1.start()  ──► JVM asks OS to spawn Thread ──► OS assigns Stack &amp; PC ──► Thread executes run() asynchronously
t1.run()    ──► Normal synchronous method call executed directly on current thread (e.g., main)
```
#### **What Happens Internally When `.start()` is Called?**
1. **OS Request:** The JVM requests the Operating System kernel to create a brand-new native execution thread.
2. **Memory Allocation:** The OS provisions dedicated resources for the new thread, including its own **private Stack** and a **Program Counter (PC) register**.
3. **Queue Placement:** The thread is placed into the CPU scheduler queue.
4. **Asynchronous Execution:** When assigned CPU time, the thread invokes the target `run()` method concurrently with the caller thread.

#### **What Happens If You Call `.run()` Directly?**
- Calling `t1.run()` executes `run()` as a **standard synchronous method call** on the calling thread (such as the `main` thread).
- The OS creates no new thread, allocates no independent stack or program counter, and code executes sequentially within the existing thread context.

#### **Can a Thread Be Started Twice?**
- **No.** Calling `.start()` more than once on the same `Thread` instance throws an `IllegalThreadStateException`.
- Once a thread initiates and completes its lifecycle, it cannot be restarted or reused.

### **4. Thread Identity and Non-Deterministic Execution**
#### **Thread Names and IDs**

- **Obtaining Thread Name:** `Thread.currentThread().getName()` retrieves the name of the thread currently executing the instruction.
- **Default Naming:** The default main thread is named `"main"`, while secondary threads receive auto-generated names like `"Thread-0"`, `"Thread-1"`, etc. Custom names can be assigned using `.setName(String)` or constructor arguments to simplify logging and production debugging.
- **Deprecation of** **getId()** **:** The `getId()` method is deprecated in modern Java releases because thread IDs provide a false sense of sequencing or hierarchy, bearing no deterministic relationship to thread scheduling order.

#### **Why Thread Execution is Non-Deterministic**
When multiple threads (e.g., one printing even numbers and another printing odd numbers) are started concurrently, their console output interleaves unpredictably across executions.

```
Run 1: T1(2) -&gt; T1(4) -&gt; T2(1) -&gt; T2(3) -&gt; T1(6) -&gt; T2(5)
Run 2: T2(1) -&gt; T2(3) -&gt; T2(5) -&gt; T1(2) -&gt; T1(4) -&gt; T2(7)
```

This non-determinism stems from several factors outside developer control:
- **OS CPU Scheduling:** Schedulers employ complex algorithms (e.g., Round Robin, Time-Slicing, Priority Preemption) to assign CPU execution quanta.
- **Core Availability & Hardware:** The distribution of threads depends on physical CPU cores and active background processes (browser, OS tasks, recording software).
- **Context Switching:** Threads are repeatedly paused and resumed to share processing resources.
- **OS Thread Creation Latency:** Minute timing variations occur when the OS allocates thread memory and registers.

### **5. Java Thread Lifecycle (`java.lang.Thread.State`)**

The JVM formalizes a thread's state through the `Thread.State` enum, encompassing six official lifecycle states.

```
                     ┌───────────────┐
                     │      NEW      │
                     └───────┬───────┘
                             │  t.start()
                             ▼
         ┌───────────────────────────────────────┐
         │               RUNNABLE                │
         │  (Ready in Queue / Running on CPU)    │
         └───▲───────────────▲───────────────▲───┘
             │               │               │
  Lock Free/ │   Lock Wait/  │ Notify/       │ Time
  I/O Done   │   I/O Block   │ Interrupted   │ Expires
             │               │               │
      ┌──────┴──────┐ ┌──────┴──────┐ ┌──────┴────────┐
      │   BLOCKED   │ │   WAITING   │ │ TIMED_WAITING │
      └─────────────┘ └─────────────┘ └───────────────┘
                             │
                             │ run() finishes
                             ▼
                     ┌───────────────┐
                     │  TERMINATED   │
                     └───────────────┘

```

#### **Detailed Breakdown of Thread States**

1. **NEW**
- The `Thread` object has been instantiated in the Java heap (`new Thread(...)`), but `.start()` has not yet been invoked.
- The OS has not allocated thread resources or native handles.
2. **RUNNABLE**
- Initiated once `.start()` is called.
- In the JVM model, **ready** (waiting in the scheduler queue for CPU time) and **running** (actively executing bytecode on a CPU core) are consolidated under the single `RUNNABLE` state.
3. **BLOCKED**
- A thread enters this state when attempting to enter a critical section guarded by a monitor lock (such as synchronized blocks or shared objects) that is currently held by another thread.
- It also occurs when a thread is suspended awaiting raw I/O operations from user input or external devices.
- The thread returns to `RUNNABLE` once the lock is released or the I/O operation completes.
4. **WAITING**
- A thread enters indefinite waiting when explicitly directed to wait for another thread's signal (e.g., calling `wait()` or parameterless `join()`).
- It transitions back to `RUNNABLE` only after another thread calls `notify()` or `notifyAll()`.
- **Lock Behavior:** Calling `wait()` releases held monitor locks, allowing competing threads to progress.
5. **TIMED_WAITING**
- A thread enters timed waiting when paused for a bounded duration using methods like `Thread.sleep(millis)`, `join(millis)`, or `wait(timeout)`
- It returns to `RUNNABLE` automatically after the time expires, or prematurely if interrupted
- **Lock Behavior:** Calling `Thread.sleep()` pauses execution **without** releasing any acquired monitor locks
6. **TERMINATED**
- The thread has finished executing its `run()` method, either through normal return or an unhandled exception
- The legacy `stop()` method also forced an immediate jump to `TERMINATED` from any state, but it is deprecated and dangerous because it leaves shared resources corrupted.

### **6. Programmatic Inspection of Thread States**

Thread states can be monitored at runtime using `thread.getState()`:

```
public class LifecycleDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(() -&gt; {
            try {
                // Thread performs work
                System.out.println("Thread running: " +                                                                           Thread.currentThread().getName());
            } catch (Exception e) {
                e.printStackTrace();
            }
        });

        // 1. NEW State
        System.out.println("State before start(): " + t1.getState()); 
        // Output: NEW

        // 2. RUNNABLE State
        t1.start();
        System.out.println("State after start(): " + t1.getState());  
        // Output: RUNNABLE

        // Pausing the main thread lets t1 complete execution
        Thread.sleep(2000); // Throws InterruptedException

        // 3. TERMINATED State
        System.out.println("State after completion: " + t1.getState()); 
        // Output: TERMINATED
    }
}
```
