---
share: true
---
		**1. Introduction: The Four Pillars of OOP**

Object-Oriented Programming (OOP) mimics real-world entities and their characteristics in software. The four foundational pillars supporting Java OOP architecture are:

1. **Encapsulation**
2. **Inheritance**
3. **Abstraction**
4. **Polymorphism**
---

**2. Pillar 1: Encapsulation**

**A. Definition & Dual Rules**

Encapsulation comes from "en-capsule" — wrapping related components into a single protective container.

```
               ┌──────────────────────────────────────────────────┐
               │                  CLASS / OBJECT                  │
               │  ┌────────────────────┐  ┌────────────────────┐  │
               │  │  Private Data /    │  │  Public Methods /  │  │
               │  │  Fields (State)    │  │  Behaviors         │  │
               │  └────────────────────┘  └────────────────────┘  │
               └──────────────────────────────────────────────────┘
```

Encapsulation mandates two fundamental rules:

1. **Data & Behavior Bundling**: Data (variables/fields) and behaviors (methods) must be encapsulated together within an object/class.
2. **Restricted Data Access (Data Hiding)**: Direct, unrestricted access to an object's internal data from outside the class must be prohibited.

---

**B. Real-World Motivation: Bank Account Example**

- **Without Encapsulation**: If `balance` is exposed publicly, external code can execute `ba.balance = 1000000;`, modifying account funds arbitrarily.
- **With Encapsulation**: Direct modification of `balance` is forbidden. Modifications occur strictly through controlled behaviors (`deposit()` and `withdraw()`) containing business logic and validations.

```
// Encapsulated Class
class BankAccount {
    private double balance; // Data hidden from external modification

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount; // Internal modification after validation
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) { // Validation check
            balance -= amount;
        }
    }

    public double getBalance() {
        return balance; // Read-only access
    }
}
```

---

**C. Getters, Setters & Validations**

- **Getters (****getField()****)**: Public methods that return field values for read access without allowing direct mutation.
- **Setters (****setField()****)**: Public methods that update field values after running validations.
- **Validation Advantages**:
    - **Age Validation**: Restrict setting age to a realistic range (e.g., $0 < \text{age} < 120$).
    - **Roll Number Bounds**: Restrict values within valid institutional bounds.
    - **College Name Verification**: Verify college validity against external API databases before updating.

---

**3. Access Modifiers in Java**

Access modifiers dictate the visibility and accessibility of classes, variables, methods, and constructors.

| Access Modifier            | Within Same Class | Within Same Package | Subclass (Other Package) | World (Anywhere) |
| -------------------------- | ----------------- | ------------------- | ------------------------ | ---------------- |
| **private**                | **Yes**           | No                  | No                       | No               |
| **default** _(no keyword)_ | **Yes**           | **Yes**             | No                       | No               |
| **protected**              | **Yes**           | **Yes**             | **Yes**                  | No               |
| **public**                 | **Yes**           | **Yes**             | **Yes**                  | **Yes**          |

---

**Strictness Hierarchy & Production Best Practices**

$$\text{Strictness: } \text{private} > \text{default} > \text{protected} > \text{public}$$

- **Production Usage**: `private` and `public` represent the vast majority of real-world use cases24. `default` and `protected` are used sparingly.
- **Top-Level Class Restrictions**: Root classes can **only** be marked `public` or `default`. Marking a root class `private` or `protected` causes a compile-time error because a root class has no outer class or inheritance context relative to which it could be restricted. _(Nested/Inner classes are the sole exception and can be_ _private__)_.

---

**4. Java Packages & Imports**

**A. What is a Package?**

A **Package** is a directory/folder structure used to group related classes and interfaces together, avoiding naming conflicts.

```
package college; // Declares that this class belongs to the 'college' package

public class Student {
    public void print() {
        System.out.println("College Student");
    }
}
```

---

**B. Package Imports & Disambiguation**

Classes outside a package must import external classes using the `import` keyword

```
import college.Student; // Imports specific class
import school.*;         // Imports all public classes in package using wildcard (*)
```

**Resolving Naming Collisions**

When two packages (`college` and `school`) contain classes with the same name (`Student`), explicit package qualification resolves ambiguity:

```
college.Student s1 = new college.Student();
school.Student s2 = new school.Student();
```

---

**C. Inbuilt Java Packages & JVM Class Loading**

Java provides extensive standard library packages:

- **java.lang**: Core language features (e.g., `String`, `System`). Automatically imported into every Java file.
- **java.util**: Data structures & frameworks (e.g., `ArrayList`, `Scanner`).
- **java.io**: File handling and stream I/O operations.

**JVM Execution Mechanics**: When an import statement (e.g., `import java.util.ArrayList;`) is compiled, the compiler verifies class existence. At runtime, the JVM loads the compiled `.class` bytecode file of the imported class into memory.

---

**5. Pillar 2: Inheritance**

**A. Concept & IS-A Relationship**

**Inheritance** allows a child class (subclass) to acquire non-private properties and behaviors from a parent class (superclass).

- **IS-A Relationship**: Represents natural hierarchical classification.
    - _Car_ **IS-A** _Vehicle
    - _EngineeringStudent_ **IS-A** _Student
- **Syntax Keyword**: **extends**.

```
class Student { // Superclass / Parent Class
    public void markAttendance() {
        System.out.println("Attendance Marked");
    }
}

class EngineeringStudent extends Student { // Subclass / Child Class
    public void attendLab() {
        System.out.println("Lab Attended");
    }
}
```

---

**B. Core Benefits of Inheritance**

1. **Code Reusability**: Common code defined in a superclass is inherited by all subclasses without code duplication.
2. **Polymorphism Foundation**: Enables dynamic method overriding and method dispatching.

---

**C. Access Rules Across Inheritance Boundaries**

- Subclasses inherit and access `public` and `protected` superclass members directly.
- Subclasses **cannot** access `private` superclass fields or methods.

---

**6. Types of Inheritance**

```
 1. Single             2. Multilevel            3. Hierarchical           4. Multiple
   ┌───┐                 ┌───┐                    ┌───┐               ┌───┐   ┌───┐
   │ A │                 │ A │                    │ A │               │ A │   │ B │
   └───┘                 └───┘                    └───┘               └───┘   └───┘
     │                     │                      ┌─┴─┐                 └───┬───┘
     ▼                     ▼                      ▼   ▼                     ▼
   ┌───┐                 ┌───┐                  ┌───┐ ┌───┐               ┌───┐
   │ B │                 │ B │                  │ B │ │ C │               │ C │
   └───┘                 └───┘                  └───┘ └───┘               └───┘
                           │                   (Supported in Java)      (NOT Supported
                           ▼                                              for Classes)
                         ┌───┐
                         │ C │
                         └───┘
                  (Supported in Java)
```

1. **Single Inheritance**: One child class extends one parent class ($A \rightarrow B$).
2. **Multilevel Inheritance**: A child class acts as a parent for another class ($A \rightarrow B \rightarrow C$)7.
3. **Hierarchical Inheritance**: Multiple child classes extend a single parent class ($B \leftarrow A \rightarrow C$)
4. **Multiple Inheritance**: One child class attempts to extend multiple parent classes ($A, B \rightarrow C$). **NOT supported in Java for classes**.

---

**Why Multiple Inheritance is Unsupported: The Diamond Problem**

```
         ┌───┐
         │ A │ (defines show())
         └───┘
        ┌──┴──┐
        ▼     ▼
      ┌───┐ ┌───┐
      │ B │ │ C │ (both override show())
      └───┬─┴───┘
          ▼
        ┌───┐
        │ D │ (inherits from B and C)
        └───┘
```

- **The Ambiguity Scenario**: If Class `A` defines `show()`, and both Class `B` and Class `C` override `show()`, then Class `D` extending both `B` and `C` inherits two conflicting versions of `show()`.
- When `d.show()` is called, the JVM cannot resolve which parent method implementation to execute53. This runtime ambiguity is known as the **Diamond Problem**.
- **Java's Solution**: Java bans multiple class inheritance (`class D extends B, C` causes a compile error)51. Multiple inheritance is supported safely via **Interfaces**.

---

**7. The** **super** **Keyword**

**A. Definition**

While `this` stores a reference pointing to the **current object**, `super` holds a reference pointing to the **parent (superclass) object** instance.

---

**B. Three Core Uses of** **super**

```
class Student {
    String name = "Default Student";

    Student(String name) {
        this.name = name;
    }

    public void print() {
        System.out.println("Name: " + name);
    }
}

class EngineeringStudent extends Student {
    String name = "Engineering Student"; // Field Shadowing

    EngineeringStudent(String name, String parentName) {
        // 1. Invoking Parent Class Constructor
        super(parentName); // MUST be the first line in constructor
    }

    public void displayNames() {
        // 2. Accessing Parent Class Variable
        System.out.println("Parent Name: " + super.name); 
        System.out.println("Child Name: " + this.name);

        // 3. Invoking Parent Class Method
        super.print(); 
    }
}
```

1. **Accessing Parent Variables (****super.variable****)**: Accesses superclass fields when shadowed by child class fields with identical names.
2. **Invoking Parent Methods (****super.method()****)**: Calls superclass method implementations.
3. **Invoking Parent Constructor (****super(args)****)**:
    - Invokes superclass constructors during object creation.
    - **Mandatory Rule**: The `super()` call **must be the very first statement** inside the child class constructor.
4. Exact Execution Order of Blocks (Parent vs. Child)
	You have `static` blocks and `super()` in separate sections, but interviewers love asking for the exact printout order when `new Child()` is called for the first time:
	
	1. **Parent `static` block** (Runs once when Parent class loads)
	2. **Child `static` block** (Runs once when Child class loads)
	3. **Parent Instance Initializer block `{ ... }`** (Runs _after_ `super()` is invoked, _before_ Parent constructor body!)
	4. **Parent Constructor body**
	5. **Child Instance Initializer block `{ ... }`**
	6. **Child Constructor body**