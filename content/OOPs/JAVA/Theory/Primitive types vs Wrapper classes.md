---
share: true
---

What the difference?

| Concept             | **Primitive Type**             | **Wrapper Class**                        |
| ------------------- | ------------------------------ | ---------------------------------------- |
| Definition          | Basic data types (not objects) | Object representation of primitive types |
| Stored In           | **Stack memory**               | **Heap memory** (as objects)             |
| Used in Collections | ❌ Not allowed                  | ✅ Allowed (`List<Integer>`, etc.)        |
| Can Be `null`       | ❌ No                           | ✅ Yes                                    |
| Type                | Not a class                    | A class in `java.lang` package           |
| Features            | Fast & memory efficient        | Extra features like methods, null-safe   |

Java 8 primitive type 

| Primitive Type | Size    | Example            | Wrapper Class |
| -------------- | ------- | ------------------ | ------------- |
| `byte`         | 1 byte  | `byte b = 1`       | `Byte`        |
| `short`        | 2 bytes | `short s = 10`     | `Short`       |
| `int`          | 4 bytes | `int i = 100`      | `Integer`     |
| `long`         | 8 bytes | `long l = 123L`    | `Long`        |
| `float`        | 4 bytes | `float f = 3.14f`  | `Float`       |
| `double`       | 8 bytes | `double d = 2.71`  | `Double`      |
| `char`         | 2 bytes | `char c = 'A'`     | `Character`   |
| `boolean`      | 1 bit   | `boolean b = true` | `Boolean`     |
### Feature :
i) Autoboxing and Unboxing : Java **automatically converts** between primitive ↔ wrapper as needed
```
int x = 5;
Integer y = x;  // auto-converted to wrapper

Integer z = 100;
int k = z;  // auto-converted to primitive
```

ii) Java allows declaring integers across four different number systems using specific prefixes

```
int decimal     = 10;        // Base-10 (Standard) [36]
int binary      = 0b1010;    // Prefix '0b' or '0B' (Base-2) [37]
int octal       = 012;       // Prefix '0' (Base-8, digits 0-7) [38]
int hex         = 0xA;       // Prefix '0x' or '0X' (Base-16, digits 0-9, A-F) [39, 40]
```

iii)  **Underscores in Numeric Literals (** )

- Developers can place underscores inside long numbers to improve readability without affecting execution.
- _Example_: `long population = 12_34_56_789L;`

iv) Type Conversion in Java 
     ├── Implicit Conversion (Widening / Automatic) (`byte` $\rightarrow$ `short` $\rightarrow$ `int` $\rightarrow$ `long` $\rightarrow$ `float` $\rightarrow$ `double`)
     ├── Explicit Conversion (Narrowing / Manual Casting) ($$\text{TargetVariable} = (\text{TargetDataType})\ \text{SourceVariable};$$)
     │        └── Truncating Conversion (Floating-point to Integer) 
     └── Automatic Type Promotion in Expressions
### Wrapper classes Need :
```
**Collections require objects**, not primitives:
List<int> list = new ArrayList<>(); // ❌ Compile Error
List<Integer> list = new ArrayList<>(); // ✅


**Null values** (to indicate “no value”):
Integer score = null; // Valid
int score = null;     // ❌ Error

**Utility methods**:
int x = Integer.parseInt("123");       // String ➝ int
String s = Double.toString(3.14);      // double ➝ String
```

### The Wrapper Class Integer Cache Trap (`-128` to `127`)

In your Wrapper Classes section, you covered Autoboxing, but interviewers **always** ask this exact output question:

Java

```
Integer a = 100, b = 100;
System.out.println(a == b); // true!

Integer x = 200, y = 200;
System.out.println(x == y); // false!
```

- **Why?** Java caches `Integer` (plus `Byte`, `Short`, `Long`, `Character`) objects in the range **`-128` to `127`** inside an internal static array (`IntegerCache.cache`) when autoboxing (`Integer.valueOf(int)`). Values outside this range allocate a brand-new object on the Heap every time. Always use `.equals()` to compare Wrapper objects.

Summary :

| Feature                    | Primitive | Wrapper           |
| -------------------------- | --------- | ----------------- |
| Performance                | ✅ Faster  | ❌ Slower          |
| Memory usage               | ✅ Less    | ❌ More            |
| Can be null                | ❌ No      | ✅ Yes             |
| Can use methods            | ❌ No      | ✅ Yes             |
| Can be used in collections | ❌ No      | ✅ Yes             |
| Suitable for DSA           | ✅ Yes     | ✅ Yes (with care) |
