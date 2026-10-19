---
share: true
---
### **Core Foundations & Definitions**
- **Shared Resource:** Any data, variable, memory location, or object shared and accessed simultaneously across multiple threads (e.g., an object field, an in-memory list, or an account balance).
- **Critical Section:** The specific block of code that accesses and mutates a shared resource; if multiple threads execute this section concurrently without protection, data corruption can occur
- **Thread Interference:** Bugs and data inconsistencies introduced when concurrent threads overlap operations on shared resources, breaking correctness.

### **1. Race Conditions & The Atomicity Problem**
#### **What is a Race Condition?**
A **race condition** occurs when a program’s final state depends unpredictably on the non-deterministic scheduling, context switching, or order of thread execution.
#### **The Root Cause: Non-Atomic Operations**
An **atomic operation** executes as a single, indivisible unit that either completes entirely or does not happen at all, with no halfway state and no possibility of context switching mid-operation.
A statement like `count++` appears to be a single action, but it actually consists of **three separate non-atomic operations**:

1. **Read:** Fetch the current value of `count` from memory into a CPU register.
2. **Increment:** Add 1 to the register value.
3. **Write:** Save the updated value from the register back to memory.

```
Thread 1 (T1)                      Thread 2 (T2)                      Memory (count)
------------------------------------------------------------------------------------
1. Read count (0)                                                     0
       [Context Switch to T2]
                                   2. Read count (0)                  0
                                   3. Increment to 1                  0
                                   4. Write count (1)                 1
       [Context Switch to T1]
2. Increment to 1                                                     1
3. Write count (1) [OVERWRITES T2]                                    1

```

- Even though both threads incremented `count`, the final value is `1` instead of `2` because T1 overwrote T2's write with a stale read.
- The same lost-update issue occurs on multi-core systems when threads run in true physical parallelism, as both cores read `0` simultaneously from memory before writing back `1`.
- In practice, running two threads incrementing a shared counter 10,000 times each results in non-deterministic final totals significantly below 20,000 (e.g., 12,109 or 19,669).

#### **Atomic vs. Non-Atomic Examples**
- **Atomic Operations:**
    - Simple variable assignments (e.g., `int x = 2;`).
    - Object reference assignments (e.g., `Student s1 = new Student();`).
- **Non-Atomic Operations:**
    - Increment/decrement operations (`x++`, `x--`).
    - Compound assignments (`x += 5` or `x = x + 5`).
    - **Check-Then-Act operations:** Conditionals where an action depends on a prior check (e.g., `if (balance &gt;= amount) { balance -= amount; }`). If a context switch occurs after the check passes, another thread can alter the state, causing invalid actions such as account balances dropping into negative values.
    - **64-bit Writes on 32-bit JVMs (Edge Case):** 64-bit data types (`double`, `long`) can be written in two separate 32-bit operations by some JVMs, leaving them non-atomic unless declared `volatile` or synchronized.

### **2. The Visibility Problem**
#### **Definition & Hardware Cause**
**Visibility** occurs when one thread modifies a shared variable, but the updated value remains **invisible** to other threads.

- **Hardware Architecture:** Each CPU core possesses its own dedicated high-speed registers and **L1/L2 caches** to avoid slow trips to main RAM.
- When Thread 1 updates a variable on Core 1, it writes the update to its local cache and flushes it to main RAM.
- If Thread 2 on Core 2 continues reading that variable, it may repeatedly read the stale value cached locally in its own CPU cache rather than fetching the fresh value from main RAM.

```
  Core 1 (Thread 1)                    Core 2 (Thread 2)
 ┌─────────────────┐                  ┌─────────────────┐
 │ Local CPU Cache │                  │ Local CPU Cache │
 │ (flag = true)   │                  │ (flag = false)  │ ◄── Reads stale cache
 └────────┬────────┘                  └────────┬────────┘
          │ (Flushes to RAM)                   │ (Bypasses RAM)
          ▼                                    ▼
 ┌──────────────────────────────────────────────────────┐
 │                      MAIN RAM                        │
 │                   (flag = true)                      │
 └──────────────────────────────────────────────────────┘

```
#### **Code Demonstration**

```
static boolean flag = false;

// Thread 2
new Thread(() -> {
    while (!flag) {
        // Empty loop: caches flag = false, spinning forever
    }
    System.out.println("Thread 2 Finished");
}).start();

// Thread 1
new Thread(() -> {
    Thread.sleep(1000);
    flag = true; // Updated, but T2 never sees it
}).start();
```
- **Why** **System.out.println** **masks visibility:** Adding a print statement inside the `while` loop makes the loop terminate because `System.out.println()` internally uses synchronization, which forces CPU cache flushes.
- **The** **volatile** **Solution:** Declaring `static volatile boolean flag = false;` guarantees visibility by forcing every read and write to bypass CPU caches and interact directly with main RAM.
- **Important Distinction:** `volatile` **does not** solve race conditions or atomicity A `volatile int count = 0;` still suffers from race conditions during `count++` because it does not prevent interleaved multi-step execution.

### **3. The Ordering Problem (Instruction Reordering)**
#### **Definition & Root Cause**
**Instruction Reordering** refers to the compiler and CPU reordering program instructions to optimize execution pipelines and performance.
- **Single-Thread Guarantee (As-If-Serial):** The compiler and CPU only guarantee that reordering will not alter the outcome of a **single-threaded** execution.
- **Multi-Thread Failure:** The runtime does not evaluate dependencies across threads.

#### **Reordering Failure Scenario**

```
// Initial values: x = 0, flag = false

// Thread 1:
x = 10;
flag = true;

// Thread 2:
if (flag) {
    System.out.println(x);
}
```

- **Expected Flow:** If `flag` is `true`, `x` must be `10`.
- **Reordered Execution:** Because `x = 10` and `flag = true` are independent, the compiler or CPU may execute `flag = true` first.
- If a context switch occurs right after `flag = true`, Thread 2 checks `flag`, finds it `true`, and prints `x = 0` (the default/un-updated value).
- These bugs are non-deterministic, making them difficult to reproduce or debug in production.

### **Summary: Problems vs. Solutions**

| Problem                            | Root Cause                                                                                                      | Symptom                                                                                   | Fixed By                                                                                                                               |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Race Condition / Non-Atomicity** | Operations take multiple steps (`read-modify-write`, `check-then-act`) and get interrupted by context switches. | Lost updates, corrupted or invalid application state.                                     | `synchronized` blocks/methods, `AtomicInteger`/`AtomicLong` (CAS algorithm), Explicit Locks. _(Note:_ _volatile_ _does NOT fix this)_. |
| **Visibility**                     | CPU caches retain local copies of variables instead of syncing with main RAM.                                   | One thread never sees changes committed by another thread.                                | `volatile` keyword, `synchronized` blocks (flushes cache on monitor release).                                                          |
| **Ordering**                       | Compiler and CPU reorder independent instructions for performance.                                              | Code runs in an unexpected sequence across threads, reading partially-initialized states. | `volatile` keyword (creates memory barriers), `synchronized` blocks.                                                                   |
