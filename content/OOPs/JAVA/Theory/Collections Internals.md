---
share: true
---

1. **`HashMap` Internal Working (Java 8+):**
    - **Structure:** An array of `Node<K,V>[] table` (buckets), default initial capacity $= 16$, load factor $= 0.75$ (resizes/doubles to $32$ when $16 \times 0.75 = 12$ elements are inserted).
    - **`put(K, V)` Flow:** Computes `hash = (h = key.hashCode()) ^ (h >>> 16)` (XORs high 16 bits with low 16 bits to reduce collisions) $\to$ finds bucket index `(n - 1) & hash`.
    - **Collision Handling:** If bucket is empty, inserts node via CAS/direct assignment. If collision occurs, chains nodes as a **Singly Linked List** (checking `equals()` along the way to overwrite duplicate keys).
    - **Treeification Threshold:** If a single bucket's linked list grows beyond **8 nodes** (`TREEIFY_THRESHOLD = 8`) AND the total array capacity is **$\ge 64$** (`MIN_TREEIFY_CAPACITY = 64`), that bucket converts into a **Balanced Red-Black Tree**, improving worst-case lookup from $O(N)$ to **$O(\log N)$**. (If bucket shrinks back to $6$ nodes during removals, it untreeifies back to a Linked List).
        
2. **`ArrayList` vs. `LinkedList`:**
    
    - `ArrayList` uses a dynamic Object array (default size $10$, grows by **$1.5\times$** via `oldCapacity + (oldCapacity >> 1)`). Fast $O(1)$ random access and CPU cache-friendly.
    - `LinkedList` uses a Doubly Linked List (24+ bytes of node pointer overhead per element, poor cache locality).
    
3. **Fail-Fast vs. Fail-Safe Iterators:**
    
    - **Fail-Fast (`ArrayList`, `HashMap`):** Maintains an internal `modCount` counter. If the collection is structurally modified while iterating (except via the iterator's own `remove()` method), it immediately throws `ConcurrentModificationException`.
    - **Fail-Safe (`CopyOnWriteArrayList`, `ConcurrentHashMap`):** Operates on a snapshot/clone or concurrent view of the underlying array and never throws `ConcurrentModificationException`.
    
4. **"Is Java Pass-by-Reference or Pass-by-Value?"**
    
    - **Strict Answer:** Java is **strictly Pass-by-Value** for everything.
    - For primitives, it passes a copy of the value. For objects, it passes **a copy of the reference pointer by value**.
    - _Proof:_ If you pass `Student s1` into `void change(Student s)` and do `s.name = "Bob"`, it modifies the heap object. But if you do `s = new Student("Charlie")` inside the method, the caller's original `s1` reference in `main()` still points to the old object!
    
5. **`Serializable` & the `transient` Keyword:**
    
    - `Serializable` is a Marker Interface (like `Cloneable`) that converts a Heap object into a byte stream (for saving to disk, Redis cache, or sending over a network).
    - Marking a field **`transient`** (e.g., `private transient String password;`) tells the JVM to **skip** that field during serialization (it restores as `null` or `0` upon deserialization). `static` fields are also never serialized because they belong to the class, not the object.