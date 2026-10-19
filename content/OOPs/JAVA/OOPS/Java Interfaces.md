---
share: true
---
## **1. What is an Interface?**

- **Definition**: An interface defines **what an object can do** without specifying **how it does it**.
- **Blueprint of Behavior**: Unlike a class (which is a blueprint for an object), an interface acts as a **blueprint of behavior**.

```
interface Car { 
void drive(); // Implicitly public and abstract 
} 

class Thar implements Car { 
	@Override 
	public void drive() { // Must be explicitly declared 'public' System.out.println("Thar is driving"); 
	} 
}
```

### **Key Benefits**

- **Loose Coupling**: Calling code remains decoupled from specific implementations.
- **Foundation for Frameworks**: Forms the architectural backbone for Clean Architecture and **Dependency Injection** in Spring Boot.

## **Variables in Interfaces**

You can define variables inside an interface, but they follow strict rules:

```
interface MathConstants {
    double PI_VALUE = 3.14; // Implicitly public static final
}
```

- **Implicit Modifiers**: Interface variables are automatically **public static final** (constants).
- **Initialization Requirement**: Must be assigned a value at declaration.
- **No Instance Fields**: Non-static instance fields cannot exist because interfaces cannot be instantiated.
- **Usage**: Can be accessed directly via the interface name (`MathConstants.PI_VALUE`) across the application.

## Diamond Problem with Default Methods & Priority Rules
### **Scenario 1: Ambiguous Default Methods**

If a class implements two interfaces (`B` and `C`) that both declare an identical `default` method signature:

```
interface B { default void fun() { System.out.println("B"); } }
interface C { default void fun() { System.out.println("C"); } }

class D implements B, C {
    // COMPILER ERROR! D must explicitly override fun()
    @Override
    public void fun() {
        B.super.fun(); // Explicitly calls B's default implementation
        C.super.fun(); // Explicitly calls C's default implementation
    }
}
```

- **Rule**: The compiler forces class `D` to explicitly override the conflicting method. You can invoke specific parent defaults using `InterfaceName.super.methodName().

### **Scenario 2: The "Class Wins" Priority Rule**

If a class extends a parent class **and** implements an interface, and both contain a method with the same signature:

```
interface A { default void fun() { System.out.println("Inside A Interface"); } }
class B { public void fun() { System.out.println("Inside B Class"); } }

class C extends B implements A {
    // No error! B's implementation takes priority automatically.
}
```

- **Java Resolution Priority Rule**: The implementation in the **super class always wins** over interface default methods

- **Aggregation (Weak HAS-A):** `Department` has `Professor`s. If the `Department` object is destroyed, the `Professor` objects still exist independently.
    
- **Composition (Strong HAS-A / Part-Of):** `House` has `Room`s (or `Human` has `Heart`). The child object cannot exist without the parent; if the parent is destroyed, the part is destroyed too