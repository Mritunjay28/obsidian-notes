---
share: true
---
#### **A. What is Abstraction?**

- **Definition**: Abstraction is the process of focusing on **what** an entity/object does while ignoring **how** it achieves that behavior internally.
- **Human Cognition Analogy**: Human society operates entirely on mental abstractions to process a complex world:
    - **Car**: A driver knows how to drive, steer, accelerate, and apply brakes without needing to understand internal combustion engines, fuel gauge sensors, or the physics of brake pad friction.
    - **ATM Machine**: A user relies on functions like `deposit()`, `withdraw()`, and `checkBalance()` without needing to know the underlying electronic circuitry or server network protocols.
- **OOP Paradigm Alignment**: Object-Oriented Programming does not simulate raw physical reality down to its atomic details; rather, it simulates **human perception and mental models of reality**.

#### **B. The Two Core Rules of Abstraction**

1. **Model Only What is Necessary**: Include only the attributes and behaviors essential for the application's domain while omitting irrelevant complexity[.
2. **Hide Implementation Details**: Expose simple control interfaces (e.g., buttons, pedals, methods) so consumers can use functionality without relying on or needing internal code mechanics.

---

### **2. Abstraction Classification in Java: Low-Level vs. High-Level**

Java implements abstraction across two primary levels:

```
                     Abstraction in Java
           ┌──────────────────┴──────────────────┐
           ▼                                     ▼
  Low-Level Abstraction                High-Level Abstraction
(Concrete Classes &amp; Access Modifiers)    (Abstract Classes &amp; Interfaces)
  • Hides method internals             • Separates WHAT from HOW
  • Keeps data encapsulated            • Decouples caller from runtime types

```

#### **A. Low-Level Abstraction**

- **Mechanism**: Implemented via standard **Concrete Classes** and access modifiers[.
- **Behavior**: Hides internal method logic inside class files. Calling `c.start()` or `c.accelerate()` executes functionality without requiring the caller to read or manage the internal lines of code[.

#### **B. High-Level Abstraction**

- **Mechanism**: Implemented via **Abstract Classes** and **Interfaces**.
- **Behavior**: Strictly separates **What** a component does from **How** it executes it. It enables loose coupling by allowing parent reference variables to point to diverse subclass implementations dynamically at runtime.

---

### **3. Abstract Classes (** **abstract** **Keyword)**

#### **A. Definition & Anatomy**

An **Abstract Class** is a restricted class declared with the `abstract` keyword that serves as a generic parent template for a family of related subclasses.

```
// Abstract Parent Class
abstract class Car {
    // Concrete Method (Defined)
    void start() {
        System.out.println("Car started");
    }

    // Abstract Methods (Declared only, no body)
    abstract void accelerate();
    abstract void applyBreak();
}

```

#### **B. Rules Governing Abstract Classes**

1. **Abstract Methods**: Declared with the `abstract` modifier and terminated with a semicolon `;` without a method body `{}.
2. **Class-Level Modifier Rule**: If a class contains at least **one abstract method**, the entire class **must be declared** **abstract
3. **No Direct Instantiation**: Abstract classes **cannot be instantiated** using the `new` keyword (`Car c = new Car();` triggers a compile-time error) because undefined abstract methods cannot be executed directly.
4. **Mandatory Subclass Implementation**: Any concrete subclass extending an abstract class **must override and define all inherited abstract methods**, or else the subclass itself must be marked `abstract.

```
class FuelCar extends Car {
    @Override
    void accelerate() {
        System.out.println("Fuel Car is accelerating via combustion engine");
    }

    @Override
    void applyBreak() {
        System.out.println("Fuel Car stopped via hydraulic breaks");
    }
}

```

---

### **4. Interfaces (** **interface** **Keyword)**

#### **A. Definition & Mechanics**

An **Interface** in Java is a pure abstraction specification that defines a contract of behaviors without containing instance state or standard method implementations.

```
// Interface Definition
interface Flyable {
    void fly(); // Implicitly public and abstract
}

// Concrete Implementation
class Aeroplane implements Flyable {
    @Override
    public void fly() {
        System.out.println("Aeroplane flying using jet engines");
    }
}

```

#### **B. Key Rules & Properties**

- **Keywords**: Defined using `interface` and inherited using the **implements** keyword.
- **Implicit Modifiers**: Methods declared in an interface are implicitly `public` and `abstract` by default.
- **Public Visibility Requirement**: When implementing interface methods inside a class, the overriding methods **must be explicitly declared** **public** to avoid reducing visibility.
- **No Direct Instantiation**: Cannot be instantiated directly with `new`.

---

### **5. Abstract Class vs. Interface: Architectural Decision Criteria**

```
             Architectural Purpose &amp; Design Intent
 ┌──────────────────────────────────┬──────────────────────────────────┐
 │         Abstract Class           │            Interface             │
 ├──────────────────────────────────┼──────────────────────────────────┤
 │ Represents a Family of Objects   │ Represents Roles / Capabilities  │
 │ (IS-A Relationship)              │ / Contracts (CAN-DO / -able)     │
 ├──────────────────────────────────┼──────────────────────────────────┤
 │ E.g., `FuelCar` IS-A `Car`       │ E.g., `Bird` and `Aeroplane`     │
 │ Shares parent state and code     │ both implement `Flyable`         │
 └──────────────────────────────────┴──────────────────────────────────┘

```

| Feature               | Abstract Class                                                    | Interface                                                                                        |
| --------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Design Intent**     | Models a **Family of similar objects** sharing state/code.        | Defines **Roles, Capabilities, or Contracts** across potentially unrelated classes.              |
| **Naming Convention** | Noun forms (e.g., `Car`, `Animal`, `Student`)                     | Adjectives / Verbs with `-able` suffix or `I` prefix (e.g., `Flyable`, `Runnable`, `Comparable`) |
| **Method Types**      | Can contain both **concrete (defined)** and **abstract** methods[ | Traditionally contains **pure abstract methods**                                                 |
| **Inheritance Model** | Supports **Single Class Inheritance** (`extends`)                 | Supports **Multiple Interface Inheritance** (`implements A, B`)                                  |

---

### **6. Abstraction vs. Encapsulation (Technical Interview Comparison)**

| Aspect           | Encapsulation                                                                                                    | Abstraction                                                             |
| ---------------- | ---------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Core Goal**    | **Data Security & Protection**.                                                                                  | **Complexity Reduction & Implementation Hiding**                        |
| **Primary Rule** | Bundles data + methods into a unit and restricts direct field access via access modifiers (`private`, `public`). | Exposes **What** an object does while hiding **How** it is implemented. |
| **Focus Area**   | How data is kept safe from unauthorized external mutation.                                                       | How complex internal logic is hidden behind simpler interfaces.         |

---

### **7. Polymorphism ("Many Forms")**

Polymorphism allows a single command, method, or object reference to exhibit different behaviors based on inputs or runtime context.

```
                         Polymorphism
           ┌──────────────────┴──────────────────┐
           ▼                                     ▼
Compile-Time Polymorphism               Runtime Polymorphism
  (Static / Method Overloading)         (Dynamic / Method Overriding)
  • Resolved during compilation         • Resolved at runtime via Heap objects
  • Based on argument signature         • Based on dynamic method dispatch

```

---

#### **A. Compile-Time Polymorphism (Method Overloading)**

- **Mechanism**: Multiple methods in the same class share the **same method name** but have **different parameter signatures** (number, type, or order of arguments).
- **Resolution**: The Java compiler determines which method to invoke at compile time based on the passed arguments[.

```
class Human {
    void run() {
        System.out.println("Running at 2 km/h"); // Default pace
    }

    void run(boolean isDogBehind) {
        if (isDogBehind) {
            System.out.println("Running at 5 km/h"); // Accelerated pace
        }
    }
}

```

---

#### **B. Runtime Polymorphism (Method Overriding)**

- **Mechanism**: A subclass provides its own specific implementation of a method already defined in its parent class or interface
- **Resolution**: Resolved dynamically at runtime using **Dynamic Method Dispatch** based on the actual object instantiated in Heap memory (`Parent p = new Child()`).

```
abstract class Animal {
    abstract void run();
}

class Dog extends Animal {
    @Override
    void run() { System.out.println("Dog runs on 4 legs"); }
}

class Human extends Animal {
    @Override
    void run() { System.out.println("Human runs on 2 legs"); }
}

// Execution
Animal a = new Dog(); // Parent reference, Child object
a.run(); // Executes Dog's run() at runtime [53, 55]

```

---

### **8. Special Keywords & Restrictions in Method Overriding**

1. **static** **Methods**: **Cannot be overridden. Static methods belong to the class, not individual instance objects. When a static method is called on a reference variable (`Parent p = new Child()`), the reference type (`Parent`) determines execution at compile time (known as _Method Hiding_).
2. **private** **Methods**: **Cannot be overridden** because they are encapsulated within the declaring class and invisible to subclasses.
3. **final** **Methods**: **Cannot be overridden. Marking a method `final` locks its implementation permanently.
4. **final** **Classes**: **Cannot be extended/subclassed** at all.
5. **Variables / Fields**: **Variables are NOT Polymorphic in Java. Variable resolution is determined strictly by the reference variable's declared type at compile time, not by the Heap object.

```
class A { int x = 10; }
class B extends A { int x = 20; }

A obj = new B();
System.out.println(obj.x); // Prints 10 (determined by reference type A, NOT object B) [61, 65]

```