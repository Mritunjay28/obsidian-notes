---
share: true
---
Java provides multiple mechanisms to initialize a stream:
1. **From Collections**:
	- Sequential stream: `collection.stream()` (available across all classes implementing the `Collection` interface: `List`, `Set`, `Queue`).
	- Parallel stream: `collection.parallelStream()` for multithreaded processing.
2. **From Arrays**:
	- `Arrays.stream(array)` converts a primitive or object array into a stream.
3. **Static Factory Methods**:
	- `Stream.of(T... values)` creates a stream from a comma-separated list of items
	- `Stream.empty()` generates an empty stream, useful for avoiding `null` references

### Streams API: Intermediate (Lazy) vs. Terminal Operations

Your Stream section stops right after initialization (`collection.stream()`). Add the execution mechanics:

- **Why Streams are Lazy:** Intermediate operations (`filter`, `map`, `flatMap`, `sorted`, `distinct`) **do not execute at all** until a **Terminal Operation** (`collect`, `forEach`, `reduce`, `count`, `findFirst`, `anyMatch`) is invoked.
- **`map()` vs. `flatMap()`:** `map()` transforms 1 element into 1 element ($1 \to 1$); `flatMap()` transforms 1 element into a Stream of elements and flattens nested structures like `List<List<Integer>>` into a single `Stream<Integer>` ($1 \to N$).
- **Single-Use Rule:** A Stream cannot be reused once a terminal operation is called; doing so throws `IllegalStateException`.