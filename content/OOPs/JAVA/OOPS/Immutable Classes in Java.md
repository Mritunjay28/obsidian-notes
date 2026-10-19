---
share: true
---
v## **Why Use Immutable Classes?**

1. **Thread Safety**: Immutable objects are inherently thread-safe Multiple threads can access the same object concurrently without risk of race conditions or data corruption.
2. **Security & Reliability**: The internal state cannot be accidentally or maliciously tampered with after creation
3. **Hash Consistency**: Because the internal state never changes, hash codes remain constant, making immutable objects ideal keys for hash-based collections such as `HashMap` or `HashSet`.

## **The Mutable Reference Problem (Shallow Copy Issue)**

If an immutable class contains fields that refer to **mutable objects** (e.g., a custom `College` object with mutable fields), the standard rules alone are insufficient.

## **Why Basic Immutability Fails for Custom Objects**

- **Reference Leakage in Constructor**: Passing a mutable object into the constructor directly (`this.college = college`) allows the caller to retain a reference to that object and modify it externally.
- **Reference Leakage in Getter**: Returning the field directly in a getter method (`return this.college`) hands out the direct reference pointer stored in heap memory.
- **Consequence**: External code can execute `s1.getCollege().setName("New Name")`, altering the internal object state. This pointer copy is known as a **Shallow Copy**.

 **Solution: Defensive Copying (Deep Copy)**

To handle mutable reference fields safely, perform **defensive copying** in both the constructor and getter methods. This technique creates a **Deep Copy** by instantiating a completely new object in heap memory rather than sharing internal references.

 **Implementing Defensive Copying**

1. **Defensive Copy in Constructor**: Create a brand-new object using the incoming object's values rather than assigning its reference directly.
2. **Defensive Copy in Getter**: Create and return a new instance containing the field's current state rather than returning the internal reference pointer.

### **Deep Copy Code Example**

```
// Mutable Helper Class
class College {
    String name;
    String address;

    public College(String name, String address) {
        this.name = name;
        this.address = address;
    }
}

// Immutable Enclosing Class
public final class Student {
    private final int age;
    private final String name;
    private final College college; // Mutable Reference Field

    public Student(int age, String name, College college) {
        this.age = age;
        this.name = name;
        // Defensive copy in constructor
        this.college = new College(college.name, college.address);
    }

    public int getAge() { return age; }
    public String getName() { return name; }

    // Defensive copy in getter
    public College getCollege() {
        return new College(this.college.name, this.college.address);
    }
}

```

---

## **6. How Fields Behave in Immutable Classes**

| Field Data Type                                      | Default Behavior              | Handling Strategy                                        |
| ---------------------------------------------------- | ----------------------------- | -------------------------------------------------------- |
| **Primitives** (`int`, `double`, etc.)               | Stored directly in memory.    | Standard `private final` fields.                         |
| **Strings** (`String`)                               | Inherently immutable in Java. | Safe to store and return directly.                       |
| **Custom Mutable Objects** (`College`, `Date`, etc.) | Mutable by default.           | Requires **Defensive Copying** in constructor & getters. |

