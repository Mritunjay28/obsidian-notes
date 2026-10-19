---
share: true
---
1) New Keyword :
   The 'new' keyword dynamically allocates(that is, allocates at run time)memory for an object & returns a reference to it.
   ==When an object is created using a new keyword, then space is allocated for the variable in a heap, and the starting/reference address is stored in the stack memory.==

2) **Static** Keyword :
   "This belongs to the **class**, not to any specific object.” So you can **access it without creating an object** of the class."
   
3) **The** **this** **Keyword** :
   `this` is a reserved keyword in Java that stores a reference pointing to the **current object** being created or executed
   
4) **The** **final** **Keyword (Constants & Immutability)**:
   Marking a variable as `final` locks its value, making it an unmodifiable **Constant**

| Element                   | Description                                     |
| ------------------------- | ----------------------------------------------- |
| `static` field (variable) | Shared among all objects (like a global var)    |
| `static` method           | Can be called without object                    |
| `static` block            | Runs once when class is loaded (initialization) |
| `static` class (inner)    | Can be created without outer class instance     |

eg:

```
Static Variables (Fields) :
class Student {
    static String college = "IIIT";  // shared by all // static (class-level)
    String name; // instance (object-level)

    Student(String name) {
        this.name = name;
    }

    void display() {
        System.out.println(name + " - " + college);
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("Alice");
        Student s2 = new Student("Bob");

        s1.display();  // Alice - IIIT
        s2.display();  // Bob - IIIT
    }
}


Static Methods :
class MathUtil {
    static int square(int x) {
        return x * x;
    }
}

public class Main {
    public static void main(String[] args) {
        System.out.println(MathUtil.square(5));  // 25
    }
}

### Rules:
- Can’t access **non-static fields(Variable)** directly
- Can be called **without an object**
  
  
  Static Blocks:- Used for **initializing static variables** or one-time setup (e.g., DB connection loading)
  class App {
    static {
        System.out.println("Static block runs once when class loads");
    }

    public static void main(String[] args) {
        System.out.println("Main method runs");
    }
}

Static Nested Class:- Unlike normal inner classes, **static nested classes do not need an instance** of the outer class.
class Outer {
    static class Inner {
        void display() {
            System.out.println("Static inner class");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.Inner obj = new Outer.Inner();
        obj.display();
    }
}



```
### Things to Watch :
- `static` methods **can’t use** `this` **or** `super`
- Static blocks run **before** `main()`
- Don’t overuse static — it **breaks OOP if abused** (e.g., global state)


# **Why** **main** **is Static in Java**

#### **Method Signature**: `public static void main(String[] args)`
- **The Problem Without** **static**: If `main` were a non-static instance method, the JVM would have to instantiate an object of the execution class (`new Demo4()`) before invoking `main()`.
- **The Solution**: Marking `main` as `static` allows the JVM to execute `ClassName.main()` directly upon launching, without requiring prior object creation.

### **Command-Line Arguments (** **String[] args** **)**
- **Definition**: The `String[] args` parameter in the `main` method captures text arguments passed from the command line/terminal when invoking the Java runtime
