---
share: true
---

1. Fundamentals of Lock-Free Concurrency & Atomic Variables

- **Limitations of Non-Atomic Operations**:
    - Compound operations such as `count++` consist of three distinct underlying steps: reading memory, incrementing the temporary value, and writing the value back to memory.
    - Context switching mid-operation between threads causes race conditions and lost updates.
    - While `synchronized` blocks and explicit locks fix race conditions, they introduce heavy performance overhead and thread blocking.
- **Definition of Atomic Variables**:
    - An atomic operation is an **"all-or-nothing"** execution unit executed without mid-operation context switching.
    - Standard Java atomic classes reside in `java.util.concurrent.atomic` (`AtomicInteger`, `AtomicLong`, `AtomicBoolean`, `AtomicReference`).
    - **Inherent Visibility**: Atomic variables provide internal visibility guarantees equivalent to the `volatile` keyword, eliminating the need to explicitly write `volatile`.

---

2. Standard Atomic Classes & Methods

- **AtomicInteger** **/** **AtomicLong**:
    - `get()`, `set(value)`.
    - `incrementAndGet()` ($\text{++}x$), `getAndIncrement()` ($x\text{++}$).
    - `decrementAndGet()` ($--x$), `getAndDecrement()` ($x--$).
    - `addAndGet(delta)`, `getAndAdd(delta)`.
    - `AtomicLong` is ideal for unique ID generation without lock contention.
- **AtomicBoolean**:
    - Commonly used for shared thread flags (e.g., status flags in Producer-Consumer models) without lock overhead.
- **Critical Rule on Combining Operations**:
    - Combining multiple individually atomic calls (e.g., `if (count.get() > 4) count.incrementAndGet();`) is **not atomic** as a whole and causes race conditions.

---

3. `AtomicReference` & Compare-and-Set (CAS)

- **Check-Then-Act Race Condition**:
    - Reference assignments like checking `if (seat == null) seat = name;` suffer from "check-then-act" flaws where multiple threads pass the check simultaneously and overwrite each other.
- 
```
AtomicReference<T>  & compareAndSet():
```

    - Wraps object references for atomic updates using `compareAndSet(expectedValue, newValue)`.
    - Compares current reference against `expectedValue`; if equal, updates to `newValue` and returns `true`. Otherwise, leaves reference untouched and returns `false`.

---

4. Lock-Free CAS Retry Loop Pattern

- **Concept**:
    - When multiple threads attempt concurrent updates, standard CAS causes failing threads to return `false`.
    - To ensure all updates are recorded without losing data, threads run a **CAS Retry Loop**.
- **Idiomatic Retry Loop Code Pattern**:

```
int currentCount, finalCount;
while (true) {
    currentCount = totalCount.get(); // Capture expected value
    finalCount = currentCount + 1;  // Compute target value
    if (totalCount.compareAndSet(currentCount, finalCount)) {
        return; // Success, exit loop
    }
    // Conflict detected: retry loop with fresh currentCount
}
```

- **Internal Java Implementation**: Higher-level atomic methods like `AtomicInteger.incrementAndGet()` internally execute this exact CAS retry loops.

---

5. `AtomicReferenceArray`

- Provides atomic operations over array elements using index positions (`get(index)`, `set(index, val)`, `compareAndSet(index, expect, update)`).
- Enables thread-safe resource allocation arrays (e.g., multi-seat reservation systems) without locking the entire array.

---

6. Lock-Free vs. Lock-Based Concurrency Comparison

| Feature      | Lock-Based (`synchronized` / `Lock`)                                                | Lock-Free CAS (`Atomic*`)                                               |
| ------------ | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Mechanic** | Threads block in a waiting queue; thread enters critical section while others wait. | All threads attempt execution simultaneously; 1 succeeds, others retry. |
| **Overhead** | Higher thread context-switching and blocking overhead.                              | Lower overhead; non-blocking execution.                                 |
| **Scope**    | Suitable for complex multi-statement critical sections.                             | Best for localized state/variable updates and counters.                 |

---

7. Hardware-Level Guarantees for CAS

- **CPU Instruction Level**:
    - Processors execute CAS via atomic hardware instructions (e.g., `LOCK CMPXCHG` on x86) with a bus lock signal to the system memory controller.
- **Physical/Electrical Impossibility of True Parallel Collision**:
    - Even during multi-core parallel execution, the memory controller operates as a physical circuit (logic gates/flip-flops).
    - Electrical signals cannot pass through hardware at the exact same physical instant—there is always a physical transmission gap (e.g., 1 CPU cycle or picosecond delay).
    - Consequently, the memory controller deterministically processes one CAS request prior to the other.

---

8. The ABA Problem & Versioning Solution

- **The Problem**:
    - Thread $T_1$ reads value $A$ and is context-switched.
    - Thread $T_2$ changes the value from $A \rightarrow B$, then back $B \rightarrow A$.
    - Thread $T_1$ resumes and runs `compareAndSet(A, newVal)`. Since the value reads $A$, CAS succeeds despite missing intermediate state mutations.
- **Impact**:
    - Irrelevant for simple numerical counters.
    - Critical bug source when object state or reference history matters (e.g., lock-free stacks or linked nodes).
- **Solution (Stamp / Versioning)**:
    - Pair the value with an incrementing version counter: $(A, v_1) \rightarrow (B, v_2) \rightarrow (A, v_3)$.
    - `AtomicStampedReference` compares both value AND version stamp. Because $(A, v_1) \neq (A, v_3)$, CAS detects the modification history and correctly retries.

