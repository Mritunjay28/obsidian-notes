---
share: true
---
two distinct mechanisms for creating string instances:
```
// 1. Literal approach
String s1 = "Hello"; 

// 2. new operator approach
String s2 = new String("Hello"); 
```

### **Why Java Strings are Immutable**
1. **Security:** Strings store sensitive parameters such as usernames, passwords, file paths, and database URLs. Immutability ensures these values cannot be modified maliciously or accidentally across callers
2. **String Pool Integrity:** Pooling relies entirely on the premise that sharing an instance across multiple variables is safe If strings were mutable, changing a value via one reference would unintentionally corrupt all other references pointing to that pooled literal.
3. **Thread Safety:** Immutable objects are inherently free from race conditions, making strings completely thread-safe without requiring locks or synchronization blocks.
4. **Hash Code Caching:** Hash-based data structures (like `HashMap` and `HashSet`) depend on stable hash keys. Because a string cannot change, its hash code is guaranteed to remain identical throughout its lifecycle

## **Compile-Time Constants vs. Runtime Evaluations**

A critical rule governs where string objects are allocated:
> **Only compile-time constants are placed in the String Pool. Strings constructed or evaluated dynamically at runtime are allocated in standard Heap memory.**

- **Case 1 (Compile-Time Resolution):**
```
String s1 = "JA" + "VA"; // Resolved by the compiler directly into "JAVA"
String s2 = "JAVA";
System.out.println(s1 == s2); // true (both refer to the pooled "JAVA")
```
Because both components are string literals, the compiler concatenates them during compilation into a single pooled literal.

- **Case 2 (Runtime Dynamic Evaluation):**
```
String s1 = "JA";
String s2 = s1 + "VA"; // s1 is a variable; evaluated at runtime
String s3 = "JAVA";
System.out.println(s2 == s3); // false
```
Because `s1` is a reference variable, concatenation cannot be resolved at compile time. It evaluates at runtime, allocating a new object in standard heap memory.

- **Case 3 (Direct Assignment):**
```
String s1 = "JAVA";
String s2 = s1;
System.out.println(s1 == s2); // true (simple assignment resolves at compile-time)
```
Both reference variables point to the exact same pooled literal

| Feature                     | `String`                                                    | `StringBuilder`                          | `StringBuffer`                                             |
| --------------------------- | ----------------------------------------------------------- | ---------------------------------------- | ---------------------------------------------------------- |
| **Mutability**              | **Immutable** (cannot be changed)                           | **Mutable** (modified in-place)          | **Mutable** (modified in-place)                            |
| **Thread Safety**           | **Inherently Thread-Safe**                                  | **Not Thread-Safe** (unsynchronized)[    | **Thread-Safe** (`synchronized` methods)                   |
| **Performance**             | Slower when undergoing frequent updates due to object churn | **Fastest** (no thread locking overhead) | Slower than `StringBuilder` due to synchronization locks   |
| **Introduced In**           | JDK 1.0                                                     | JDK 1.5                                  | JDK 1.0                                                    |
| **Overridden** **equals()** | Yes (compares character values)                             | No (compares references using identity)  | No (compares references using identity)                    |
| **Industry Usage**          | Standard text storage, constants, DTOs, entity keys         | **~90% of real-world string mutations**  | Rarely used (~10% legacy or specific concurrent scenarios) |
