---
share: true
---
An **immutable** object **cannot be changed** after it is created. Any “modification” creates a **new object**, not altering the original.

 ✅ Examples:
- `String`
- Wrapper classes: `Integer`, `Boolean`, etc.
- `LocalDate`, `BigInteger`, `BigDecimal` (from `java.time`, `java.math`)

 A **mutable** object **can be changed** after creation — same object, different state.

 ✅ Examples:
- `StringBuilder`
- Arrays
- `ArrayList`, `HashMap`, etc.

| Feature              | Immutable                      | Mutable                      |
| -------------------- | ------------------------------ | ---------------------------- |
| Can change data?     | ❌ No                           | ✅ Yes                        |
| Memory efficient?    | ❌ Creates new object           | ✅ Updates in place           |
| Thread-safe?         | ✅ Yes (mostly)                 | ❌ Not by default             |
| Used in HashMap key? | ✅ Good choice (fixed hashcode) | ❌ Risky                      |
| Example              | `String`, `Integer`            | `StringBuilder`, `ArrayList` |
example :
```
## Immutable
public class TestImmutable {
    public static void main(String[] args) {
        String s1 = "Java";
        s1.concat(" World");  // does nothing to s1
        System.out.println(s1);  // Output: Java

        String s2 = s1.concat(" World");
        System.out.println(s2);  // Output: Java World
    }
}

## Mutable
public class TestMutable {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Java");
        sb.append(" World");
        System.out.println(sb);  // Output: Java World
    }
}
```

### Why Make Objects Immutable

Benefits:
- **Thread-safe** without synchronization.
- **Safe as keys** in `HashMap`.
- **Simplifies debugging** (no unexpected changes).
- Helps in **functional programming style**.

To Create Own Immutable Class - Have Only Getter and Set data on object Creation with no Setters 
rules for above : 
- Mark class as `final`
- All fields should be `private final`
- No setters
- No method should expose mutable references
```
final class Person { // class final
    private final String name; // fields final
    private final int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // No setters
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

## Interview Pitfall

> ❓ "If you store an object as a key in `HashMap` and then mutate it — what happens?"

- If the object is **mutable** and its fields used in `equals()` or `hashCode()` change, the map may **fail to find the object**.
- Always use **immutable keys** in maps.