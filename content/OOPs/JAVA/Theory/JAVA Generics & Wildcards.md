---
share: true
---
Generics introduce **type parameters**—placeholders inside diamond brackets (`<T>`) evaluated when an instance is declared

```
// Generic Class Definition public class Box<T>{ 
	private T value; 
	
	public Box(T value) { 
	this.value = value; 
	} 
	public T getValue() { 
	return this.value; 
	} 
	public void setValue(T value) { 
	this.value = value; 
	} 
}
```

Wildcards in Generics (`?`, `? extends`, `? super`)

Type Parameter `<T>` vs. Wildcard `<?>`

| Characteristic          | Type Parameter `<T>`                                                                             | Wildcard `<?>`                                                                  |
| ----------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------- |
| **Declaration Scope**   | Used to declare generic classes (`class Box<T>`) and generic methods (`<T> void fun(T a, T b)`). | Represents an unknown type reference in container types (e.g., `List<?>`).      |
| **Class Declarations**  | Valid: `class Box<T> { ... }`.                                                                   | **Illegal:** `class Box<?> { ... }` causes a syntax error                       |
| **Method Declarations** | Valid: `<T> void fun(T a, T b)`.                                                                 | **Illegal:** `<> void fun(? a, ? b)` causes a syntax error                      |
| **Primary Purpose**     | Defines flexible, reusable generic types and methods.                                            | Resolves the invariance problem to allow references across generic collections. |

**Does the JVM know about Generics?** 
**No**. Generics operate purely as a **compile-time safety mechanism** and are completely erased before execution in the JVM
