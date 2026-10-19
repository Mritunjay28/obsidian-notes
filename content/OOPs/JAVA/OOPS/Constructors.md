---
share: true
---
#### **A. Constructor Overloading**

Declaring multiple constructors within the same class, differentiated by parameter lists (number, type, or order of parameters).

#### **B. Constructor Chaining (** **this()** **)**

- **Definition**: The process of calling one constructor from another constructor within the same class using the `this(...)` syntax.
- **Benefits**: Eliminates repetitive initialization code across multiple overloaded constructors.

```
class Student {
    String name;
    int age;
    int rollNumber;
    String college;

    // Constructor 1: Full Parameters
    Student(String name, int age, int rollNumber, String college) {
        this.name = name;
        this.age = age;
        this.rollNumber = rollNumber;
        this.college = college;
    }

    // Constructor 2: Name and Age only (Chained to Constructor 1)
    Student(String name, int age) {
        this(name, age, 0, "Unknown"); // Calls Constructor 1 passing default fallbacks
    }

    // Constructor 3: No Arguments (Chained to Constructor 2)
    Student() {
        this("Unknown", 0); // Calls Constructor 2
    }
}

```

#### **STRICT SYNTAX RULE for** **this()**

- When invoking another constructor using `this(...)`, **it MUST be the very first statement** inside the constructor body.
- Placing any statement before `this(...)` causes a compile-time error


### **POJO (Plain Old Java Object) & Domain Models**

**A. Definition**

A **POJO** is a simple, lightweight Java class that does not extend specialized framework classes or implement framework-specific interface

```
public class Student {
    private String name;
    private int age;

    // Constructors
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getters and Setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}

```

**B. Purpose**

POJOs are primarily used as **Model Classes** to encapsulate domain data when representing entities in database tables (e.g., ORM frameworks like Hibernate/Spring Data SQL) or transfer objects across application layers

**C. Anemic vs. Rich Domain Models**

1. **Anemic Domain Model**: A pure POJO containing strictly private fields, constructors, getters, and setters, without any business logic methods.
2. **Rich Domain Model**: A POJO that includes core domain business logic methods (e.g., `markAttendance()`) alongside its state fields and getters/setters