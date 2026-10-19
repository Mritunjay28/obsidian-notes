---
share: true
---
### **Functional Interface Fundamentals & Lambda Mechanics**
#### **A. Definition & Constraints**
- A **Functional Interface** is an interface that contains **exactly one abstract method (SAM - Single Abstract Method)
- It may contain any number of `default` or `static` methods because those provide concrete implementations[
- **The** **@FunctionalInterface** **Annotation**: An optional compiler-level annotation that ensures compile-time safety by throwing an error if a developer attempts to add a second abstract method
#### **B. Why Lambdas Require a Single Abstract Method**
- If an interface contained multiple abstract methods (e.g., `f1()` and `f2()`), the Java compiler would not know which method signature the anonymous lambda expression is attempting to implement. With exactly one abstract method, ambiguity is eliminated
#### **C. Lambda Expression Syntax Variants**
The general form is `(parameters) -&gt; expression` (where `-&gt;` represents "maps to" or "gives"):
1. **Multiple Parameters**: `(a, b) -&gt; a + b
2. **Single Parameter**: `x -&gt; x * x` (parentheses around the parameter are optional)
3. **Zero Parameters**: `() -&gt; System.out.println("Hello")` (empty parentheses required)
4. **Multi-line Block**: `(a, b) -&gt; { int sum = a + b; return sum; }` (curly braces and an explicit `return` keyword are mandatory when spanning multiple lines)
#### **D. Target Typing & Context Deduction**
- The Java compiler resolves variable types in lambdas automatically through **target typing** By inspecting the context (e.g., recognizing that `Collections.sort` on a `List.

# # Method References (`::`)

A. Core Concept
- A **Method Reference** is an ultra-compact shorthand for a lambda expression that does nothing other than pass its arguments directly through to an existing method.
- Instead of writing `x -> System.out.println(x)`, the pass-through can be written as `System.out::println`.
- _Constraint_: If any modification or custom calculation occurs on the parameter (e.g., `x -> x + 5` or `s -> s.toUpperCase() + "!"`), a method reference **cannot** be used
B. The Four Types of Method References

| Type                                   | Syntax Pattern              | Lambda Equivalent            | Method Reference Equivalent |
| -------------------------------------- | --------------------------- | ---------------------------- | --------------------------- |
| **Static Method Reference**            | `ClassName::staticMethod`   | `x -> Math.abs(x)`           | `Math::abs`                 |
| **Instance Method (Specific Object)**  | `instanceRef::methodName`   | `x -> System.out.println(x)` | `System.out::println`       |
| **Instance Method (Arbitrary Object)** | `ClassName::instanceMethod` | `str -> str.isEmpty()`       | `String::isEmpty`           |
| **Constructor Reference**              | `ClassName::new`            | `() -> new ArrayList<>()`    | `ArrayList::new`            |

## Functional Composition & Chaining Pipelines

Functional interfaces contain concrete `default` methods that permit building mathematical and logical pipelines:

A. `Function` Composition: `andThen` vs. `compose`

- **andThen** **(Left-to-Right Execution)**:
    - Syntax: `f1.andThen(f2)` executes `f1` first, then feeds its result into `f2` (equivalent to $f_2(f_1(x))$).
    - _Type Matching Rule_: The output type of the first function must match the input type of the second function $$\text{Function}\langle T, R\rangle.\text{andThen}(\text{Function}\langle R, V\rangle) \implies \text{Function}\langle T, V\rangle$$
- **compose** **(Right-to-Left Execution)**:
    - Syntax: `f1.compose(f2)` executes the argument `f2` first, then feeds that result into `f1` (equivalent to $f_1(f_2(x))$)
- **Practical Pipeline Example**:
````
Function<String, String> cleanText = ((Function<String, String>) String::trim)
    .andThen(String::toUpperCase);
String result = cleanText.apply("   hello world   "); // "HELLO WORLD"
````

B. `Predicate` Composition: Eliminating Nested `if-else`

Predicates support boolean chaining through default methods that evaluate conditions without procedural branching:
- **and(Predicate other)**: Evaluates logical AND (`&&`) across both tests.
- **or(Predicate other)**: Evaluates logical OR (`||`) across both tests.
- **negate()**: Inverts the predicate condition (`!`).
- **Domain Example**:

````
Predicate<Student> isPassed = s -> s.marks >= 40;
Predicate<Student> isAdult  = s -> s.age >= 18;
Predicate<Student> isEligible = isPassed.and(isAdult);
````

C. `Consumer` Composition

- **andThen(Consumer after)**: Executes multiple independent actions sequentially on the **same** input without producing an output:

````
Consumer<String> pipeline = printNormal.andThen(printUpperCase);
pipeline.accept("aditya"); // prints "aditya", then prints "ADITYA"
````
