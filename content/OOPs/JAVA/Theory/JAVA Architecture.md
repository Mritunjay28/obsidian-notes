---
share: true
---
**1. JDK, JRE, and JVM Architecture Hierarchy**
Java relies on a nested architectural hierarchy comprising **JVM**, **JRE**, and **JDK**12.

```
┌─────────────────────────────────────────────────────────────┐
│ JDK (Java Development Kit)                                  │
│   ├── Development Tools (javac, Debugger, JavaDoc, etc.)    │
│   └── JRE (Java Runtime Environment)                        │
│         ├── Class Libraries (Inbuilt API Methods)           │
│         └── JVM (Java Virtual Machine)                      │
│               ├── Interpreter + JIT Compiler                │
│               ├── Sandbox Security Model                    │
│               └── Automatic Garbage Collection              │
└─────────────────────────────────────────────────────────────┘
```

**A. JVM (Java Virtual Machine)**
- **Role**: The innermost runtime engine that converts platform-independent **Bytecode** (`.class` file) into platform-specific **Machine Code** (`0`s and `1`s) for the host processor.
- **Core Functions**:
    1. **Code Translation**
    2. **Security (Sandbox Model)**
    3. **Automatic Garbage Collection**
- **Platform Dependence**: While bytecode is universal, the JVM itself is platform-dependent; each OS and hardware architecture requires its own specific JVM implementation.

**B. JRE (Java Runtime Environment)**
- **Formula**: JRE = JVM + Class Libraries
- **Role**: Provides the execution environment along with built-in **Class Libraries** (e.g., standard input/output handling, system interaction methods) required to run Java applications

**C. JDK (Java Development Kit)**

- **Formula**: JDK = JRE +Development Tools
- **Role**: The complete toolkit required by developers to write, compile, debug, and run Java code.
- **Key Components**: Includes the Java compiler (`javac`), debugger, documentation generators (`javadoc`), JRE, and JVM.
---

**2. Execution Engine: Compiler, Interpreter & JIT**

**Compiler vs. Interpreter**
- **Compiler**: Translates the entire source code file into target code at once before execution.
- **Interpreter**: Translates and executes source code line-by-line in real time.

**Why Java Uses Both (Hybrid Execution Model)**
1. **Compilation Phase**: The Java compiler (`javac`) compiles `.java` source code into intermediate **Bytecode** (`.class`).
2. **Execution Phase**: The JVM converts the bytecode into native machine code at runtime.
---

**3. Java Editions: JSE vs. JEE vs. JME**
- **JSE (Java Standard Edition / Core Java)**:
    - Contains the base Java language APIs, syntax, and Object-Oriented Programming (OOP) foundation.
- **JEE (Java Enterprise Edition / Jakarta EE)**:
    - Extends Core Java with additional enterprise-grade libraries and frameworks (e.g., Spring Boot, Servlets) designed for building enterprise web applications.
- **JME (Java Micro Edition)**:
    - A lightweight edition historically used for resource-constrained devices like early mobile phones; now obsolete and largely replaced by Android development.
---
**Code Structure Example**

```
public class Demo {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

- **Compilation Command**: `javac Demo.java` $\rightarrow$ Generates `Demo.class`
- **Execution Command**: `java Demo` $\rightarrow$ Outputs `Hello World`

# Memory Architecture: Stack vs. Heap Memory

- **The Key Question**: How does Java achieve **Random Access** ($O(1)$ time complexity) when accessing any array index (e.g., `arr`1) without looping through preceding elements?
- **Core Answer**: Arrays reserve **contiguous (continuous) memory blocks** in **Heap memory**. The JVM uses a mathematical offset formula to calculate the exact physical memory address of any index instantly.

```
                  Java Memory Architecture
         ┌────────────────────────┬────────────────────────┐
         │     Stack Memory       │      Heap Memory       │
         ├────────────────────────┼────────────────────────┤
         │ • Stores Primitive     │ • Stores Non-Primitive │
         │   data types           │   objects &amp; Arrays │
         │ • Stores Reference     │ • Dynamically allocated│
         │   variables            │   via `new` keyword    │
         └────────────────────────┴────────────────────────┘
```

