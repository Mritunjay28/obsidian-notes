---
share: true
---
1. Introduction & Motivation for Multithreading

- **What is Multithreading?** It is the execution of a program using multiple threads concurrently or in parallel, rather than relying solely on a single sequential thread.
- **The "Speed Myth":** While multithreading can decrease execution time, making programs faster is only one aspect and often not the primary benefit.
- **Real-World Motivation (Responsiveness):** Modern applications perform multiple distinct tasks at the same time.
    - _Example (IDE Behavior):_ In an editor like VS Code, while a developer writes code, one background thread checks syntax and highlights compilation errors with red squiggles, while another thread suggests auto-completions.

2. Program vs. Process

| Attribute                | Program                                                              | Process                                                         |
| ------------------------ | -------------------------------------------------------------------- | --------------------------------------------------------------- |
| **Definition**           | A passive set of written instructions stored on disk                 | An active program currently being executed in RAM               |
| **Storage Location**     | Secondary storage (Hard Disk / SSD) as `.java` or `.class` bytecode. | Primary storage (Main Memory / RAM).                            |
| **Resource Consumption** | Consumes no RAM or CPU cycles.                                       | Allocated a dedicated block of RAM, CPU time, and OS resources. |
| **Execution State**      | Static / Inactive.                                                   | Dynamic / In execution.                                         |

- **Hardware Execution Flow:** The CPU interacts with high-speed registers, cache, and main RAM; it never interacts directly with a hard drive or SSD to execute code.
- **Role of the Operating System (OS):** When a user launches an application (e.g., clicking on Chrome or running `java Demo`), the OS fetches the compiled bytecode from disk and loads it into RAM, creating an independent **process**.
- **Memory Isolation:** Each process receives its own protected RAM space (e.g., 2 GB). **Processes do not share memory**: one process (e.g., Google Chrome) cannot access, read, or overwrite the memory allocated to another process (e.g., VS Code).

3. What is a Thread?

- **Definition:** A thread is a **lightweight process** or the smallest sequence of instructions that can be executed independently by the CPU.
- **Location:** Threads always reside inside a process.
- **The Main Thread:** Every Java application begins execution with at least one thread created automatically by the JVM—the **Main Thread** (`main`).
- **User-Defined Threads:** Developers can spawn additional worker threads ($T_1, T_2, \dots$) within the same process.
- **What the CPU Actually Executes:** The CPU scheduler never executes an entire process as a monolithic unit; **the CPU exclusively executes individual threads**.


4. Java & JVM Memory Architecture

OS-Level Memory Model vs. JVM Memory Model

- **Standard OS Model (Language-Independent / C++):** The OS divides process memory into Code Segment, Data Segment, Heap, Stack, and Registers.
- **JVM Model (Java-Specific):** JVM establishes its own memory organization: **Heap**, **Method Area**, **Stack**, and **Program Counter (PC)**.
    - _Why does JVM use its own model?_ To guarantee **platform independence** so Java code does not rely on underlying OS memory quirks.
    - _Mapping:_ Data and Code segments map to the JVM's **Method Area**, registers map to the thread's **Program Counter (PC)**, while Heap and Stack remain direct counterparts.

```
┌─────────────────────────────────────────────────────────────┐
│                       PROCESS (RAM)                         │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                        JVM                            │  │
│  │                                                       │  │
│  │  ┌─────────────────────────┐ ┌─────────────────────┐  │  │
│  │  │       HEAP MEMORY       │ │     METHOD AREA     │  │  │
│  │  │    (Objects, Shared)    │ │ (Metadata, Shared)  │  │  │
│  │  └─────────────────────────┘ └─────────────────────┘  │  │
│  │                                                       │  │
│  │  ┌───────────────────────┐   ┌───────────────────────┐│  │
│  │  │      MAIN THREAD      │   │       THREAD T1       ││  │
│  │  │ ┌───────┐   ┌───────┐ │   │ ┌───────┐   ┌───────┐ ││  │
│  │  │ │ Stack │   │  PC   │ │   │ │ Stack │   │  PC   │ ││  │
│  │  │ └───────┘   └───────┘ │   │ └───────┘   └───────┘ ││  │
│  │  └───────────────────────┘   └───────────────────────┘│  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

Shared vs. Thread-Private Memory

1. **Shared Among All Threads in a Process:**
    - **Heap:** Stores all created objects and instances.
    - **Method Area:** Stores loaded class definitions, method bytecode, and class metadata.
2. **Private to Each Individual Thread:**
    - **Stack:** Dedicated memory for each thread holding its active method frames, local variables, and the **operand stack** for intermediate calculations.
    - **Program Counter (PC) Register:** Keeps track of the exact address of the current or next instruction the thread must execute.

Startup Sequence of `java Demo`:

1. The OS loads the JVM into an allocated process in RAM.
2. The JVM starts its ClassLoader to locate and load the main class.
3. The JVM creates the `main` thread.
4. The JVM allocates a separate, private Stack and Program Counter (PC) for the `main` thread.
5. Execution starts from the `main` method by pushing a new stack frame onto the `main` thread's stack.

-
1. Concurrency vs. Parallelism
Concurrency (Single-Core Execution via Context Switching)
- **Single Core Limitation:** A single CPU core can physically execute only one instruction/thread at any exact point in time.
- **How Concurrency Works:** The CPU switches between active threads ($Main, T_1, \dots$) in small time slices (e.g., fractions of a second).
- **Context Switching Mechanics:**
    1. The CPU pauses the currently running thread.
    2. The intermediate execution state (partial calculation values stored in the operand stack) and the address of the next instruction (stored in the thread's PC) are saved into that thread's private stack and PC register.
    3. The CPU loads the saved PC and stack context of the next thread and begins executing it.
- **Definition of Concurrency:** Multiple threads are in progress over time, though only one thread is making progress at any single instant. Due to rapid switching, it appears to users as if everything is running simultaneously.

Parallelism (Multi-Core Execution / True Parallelism)
- **Multi-Core Capability:** When a system has multiple cores, each core can execute an independent thread simultaneously.
- **Zero Switching Overhead:** Core 1 executes the `main` thread while Core 2 runs Thread $T_1$ at the exact same physical instant without requiring context switching between them.
- **Execution Time Comparison:** If Task A and Task B each take 3 seconds:
    - _Concurrent (Single Core):_ $3\text{s} + 3\text{s} = 6\text{s}$ total.
    - _Parallel (Dual Core):_ Completed concurrently in $3\text{s}$ total.

Real-World Systems (Hybrid Approach)
- In practical setups, systems run hundreds of threads across a limited number of CPU cores.
- The OS scheduler combines **parallelism** (distributing threads across physical cores) and **concurrency** (context switching between waiting/runnable threads on each core).

6. Multitasking vs. Multithreading

| Feature                 | Multitasking                                                                                  | Multithreading                                                                                             |
| ----------------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Conceptual Level**    | Operating System (OS) level concept.                                                          | Programming / Process level concept.                                                                       |
| **Unit of Execution**   | Manages multiple independent **processes** (e.g., Chrome, VS Code, Spotify running together). | Manages multiple **threads** within a single process (e.g., auto-complete and compiler checks in VS Code). |
| **Memory Isolation**    | Processes have strictly isolated memory spaces; no shared RAM.                                | Threads within the same process share Heap and Method Area.                                                |
| **Context Switch Cost** | Heavyweight (switching address spaces and process tables).                                    | Lightweight (swaps only stack pointers and PC registers within the same address space).                    |
| **CPU Perspective**     | The CPU scheduler does not schedule processes directly.                                       | The CPU scheduler schedules and executes individual threads across available cores.                        |
