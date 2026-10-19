---
share: true
---
- A class is a blueprint used to create objects that share common properties and behavior.
- An object is an instance of a class. It represents a specific entity created from the class template.

- **Object-Oriented Programming :** It is a programming approach that organizes code into objects and classes and makes it more structured and easy to manage.

- **Class** is a user-defined data type which defines its properties and its functions. Class is the only logical representation of the data. For example, Human being is a class. The body parts of a human being are its properties, and the actions performed by the body parts are known as functions. The class does not occupy any memory space till the time an object is instantiated.(groups of objects)

- **Object** is a run-time entity. It is an instance of the class. An object can represent a person, place or any other item. An object can operate on both data members and member functions. ( an entity in real life). _Objects (non-primitive types) are always allocated on the heap, while their reference variables are stored on the stack._

- **Method** is a collection of statements that perform specific tasks and return a result to the caller. It can be declared with or without arguments, depending on the requirements. A method can take input values, perform operations, and return a result. (like a function)

![[Pasted image 20260114192850.png|Pasted image 20260114192850.png]]

example -
```
public class java {
  public static void main(String[] args) {  
    // public is an access specifier
      pen p1 = new pen(); // created a object p1 in class pen , Declaring Objects (Also called instantiating a Class)
p1.set_color("yellow");// set color through function
p1.color = "red";// directly change in color string without calling function
      // pen() is a Constructor
  }
}
class pen{
 // below given are properties of class pen 
  int tip;
  String color;
 // below given are Method of class pen
 void set_tip(int new_tip){
  tip=new_tip;
 }
 void set_color(String new_color){
  color=new_color;
 }
// Getter methods
    public int getId() { return id; }
    public String getName() { return name; }
}
```

An object mainly consists of: 

- **State**: It is represented by the attributes of an object. It also reflects the properties of an object.
- **Behavior**: It is represented by the methods of an object. It also reflects the response of an object to other objects.
- **Identity**: It is a unique name given to an object that enables it to interact with other objects.
![[image.png|image.png]]


## Fundamental Characteristics of the `Object` Class

**Why a Root Class Exists**:
1. **Common Behavior**: Provides core methods required by all objects in Java.
2. **Polymorphic Reference Holding**: An `Object` reference variable can point to an instance of any class in Java (e.g., `Object obj = new Student()`).

### **A. `toString()` Method**

- **Signature**: `public String toString()`
- **Purpose**: Returns a string representation of an object[.
- **Default Implementation**: Returns the format `ClassName@HexadecimalCode` (e.g., `Student@15db9742`).
- **Overriding**: Can be overridden to return meaningful object details (e.g., returning name and age).
- **Implicit Invocations**: `System.out.println(obj)` automatically calls `obj.toString()` internally.

### **B. `equals(Object obj)` Method**

- **Signature**: `public boolean equals(Object obj)`[
- **Purpose**: Compares two objects for equality.
- **Default Implementation**: Compares object reference addresses in memory using the `==` operator (`return (this == obj)`). Returns `false` for two distinct objects even if they contain identical values.
- **Overriding for Value Equality**: To compare field values instead of memory references, `equals()` must be overridden[.
- **Robust Overriding Steps**:
    1. **Reference Check**: If `this == obj`, return `true` immediately.
    2. **Null Check**: If `obj == null`, return `false`[
    3. **Class Type Check**: Check if runtime classes match using `getClass()` (or `instanceof`) to prevent a `ClassCastException`[
    4. **Typecasting & Field Comparison**: Typecast `obj` to the class type and compare fields

### **C. `hashCode()` Method**

- **Signature**: `public int hashCode()`
- **Purpose**: Generates an integer value (hash code) representing an object. It is used heavily in hash-based collections (`HashMap`, `HashSet`, `Hashtable`).
- **The** **equals** **and** **hashCode** **Contract**:
    1. **If two objects are equal according to** **equals()** **, their** **hashCode()** **values MUST be equal**[
    2. **If two objects have the same** **hashCode()** **, they are NOT required to be equal** (hash collisions can occur)[.
- **Requirement to Override Both**: Overriding `equals()` without overriding `hashCode()` breaks the contract and causes subtle bugs in hash-based collections.
- **Standard Overriding Options**:
    - Using `java.util.Objects.hash(field1, field2).
    - Manual calculation using non-zero initial prime multipliers (such as `17` and `31`) to ensure unique distribution[.

### **D. `getClass()` Method**

- **Signature**: `public final Class getClass()`
- **Key Constraint**: Declared as `final`, meaning it **cannot** be overridden by child classes
- **Purpose**: Returns a `Class` object representing the exact runtime type of the instance Calling `.getName()` on it returns the class name string
- **getClass()** **vs** **instanceof**:
    - `getClass()`: Checks for exact runtime class matching.
    - `instanceof`: Checks if an object is an instance of a class or any of its parent classes / subclasses.

## **E. Object Cloning: `clone()` Method**

- **Signature**: `protected Object clone() throws CloneNotSupportedException`
- **Purpose**: Creates and returns a copy of an existing object in heap memory.
- **The** **Cloneable** **Interface Requirement**:
    - A class **must** explicitly implement the `Cloneable` interface
    - If `super.clone()` is called on a class that does not implement `Cloneable`, Java throws a `CloneNotSupportedException` at runtime
- **Marker Interface Concept**: `Cloneable` is an empty interface containing no methods. It acts as a **marker interface** to flag permission for Java's runtime to allow cloning
- **Copy Type**: The default implementation in `Object` performs a **shallow copy**. For a deep copy, `clone()` must be custom-overridden