---
share: true
---
Introduction to Java Enums in **Java 5**, an **enum** **(Enumeration)** defines a predefined, fixed set of named constants.
```
public enum PaymentStatus {
    SUCCESS, FAILED, PENDING;
}
```
### **Key Advantages**
- **Compile-Time Type Safety:** A variable declared as `PaymentStatus` can **only** hold one of the values defined in the enum Passing integers, arbitrary strings, or constants from unrelated enums results in an immediate **compile-time error**
- **High Readability:** Constant names clearly convey their domain meaning directly in code



Why Not Use `int` or `String` for Constants?
 **Problems with `int` Constants**
1. **Lack of Type Safety:**
2. **Poor Readability (Magic Numbers):**
3. **No Grouping / Cross-Domain Collision:**
4. **Accidental Duplication:**

**Problems with `String` Constants**
1. **Case-Sensitivity & Typo Vulnerability:**
2. **Performance Overhead:**
3. **Risk of Arbitrary Values:**

## Under the Hood
an `enum` is **not a primitive type**; it is a **special class** managed by the compiler[
When you define:
```
enum Direction {
    NORTH, SOUTH, EAST, WEST;
}
```
The Java compiler converts it behind the scenes into:
```
public final class Direction extends java.lang.Enum<Direction>{

public static final Direction NORTH = new Direction(); 
public static final Direction SOUTH = new Direction(); 
public static final Direction EAST = new Direction(); 
public static final Direction WEST = new Direction(); 

private Direction() { }

}
```

