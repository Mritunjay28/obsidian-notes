---
share: true
---


- **Console I/O:** Reading input from the keyboard and writing output to the console screen 
- **File I/O:** Reading from and writing to files on storage 
- **Network I/O:** Exchanging data across the internet via API calls, client-server requests, and sockets 
- **Memory I/O:** Reading from and writing to internal memory buffers

Classic statement `System.out.println("Hello");` relies on three distinct Java components:
```
   System       .         out          .      println("Hello");
[Class Name]      [Static Variable]          [Method Invocation]

```
### **Component Breakdown**
1. **System** **Class:**
- Belongs to the **java.lang** package.
- The Java compiler automatically imports `java.lang.*` into every Java file, which is why `System` can be referenced without an explicit `import` statement
1. **out** **Variable:**
- Declared inside `System` as: `public static final PrintStream out;`
- Because it is declared **static**, it belongs to the `System` class and is accessed directly via `System.out` without needing to instantiate a `System` object (`new System()`)
- Marked **final** so its reference cannot be reassigned
- The runtime object referred to by `out` is an instance of **PrintStream
1. **println()** **Method:**
- A method belonging to the **PrintStream** class.
- Accepts arguments (such as a `String`), prints them to the console, and terminates the line with a newline character (`\n`)

`PrintStream` Methods & Variants
- **print()** **:** Outputs text to the console without appending a newline.
- **printf()** **:** Prints formatted strings using format specifiers (e.g., for controlling decimal places).

- **System.err** **:**
- Another `static final PrintStream` variable declared in the `System` class[.
- Used to output error messages


n Java, **all I/O operations are stream-based**

- **Definition:** A **stream** is an abstraction representing a **continuous flow of data** (similar to water flowing through a pipe)[
- **Data Unit:** Low-level Java streams operate as a **stream of bytes** (where 1 byte = 8 bits)
- **Direction of Data:**
    - **InputStream** **:** Data flows **into** the program (e.g., from the keyboard, network, or file)
    - **OutputStream** **:** Data flows **out of** the program (e.g., to the console, file, or network)

### **The Abstract Stream Hierarchy**
Both `InputStream` and `OutputStream` are **abstract classes** defined in `java.io

- **InputStream** declares an abstract `read()` method. Concrete subclasses include:
    - `BufferedInputStream` (which backs `System.in`)[
    - `FileInputStream`
    - `ByteArrayInputStream`
    - `DataInputStream
- **OutputStream** declares an abstract `write()` method Concrete subclasses include:
    - `PrintStream` (used by `System.out` and `System.err`)
    - `FileOutputStream`
    - `ByteArrayOutputStream`
    - `BufferedOutputStream

## **4.Reading Input via `System.in` and `read()`**

### **What is `System.in`?**
- Declared inside the `System` class as: `public static final InputStream in
- By default, `System.in` is tied to standard input—the **keyboard**

### **Using `System.in.read()`**
- The `read()` method reads raw data from the input stream
- **Return Type:** Returns an **int** representing the **ASCII value** of the byte read
    - Example: Pressing the key `'A'` returns the integer `65
- **Checked Exception:** Requires handling or declaring **throws IOException** from `java.io
- **Typecasting:** To retrieve the character rather than its numeric ASCII code, explicit typecasting is required: `(char) System.in.read()

### **Major Limitations of `System.in.read()`**
1. **Reads Exactly One Byte at a Time:**
- It only consumes a single byte from the OS input buffe
1. **Requires Manual Loop Processing:**
- To read a full string, the programmer must loop through `System.in.read()`, appending cast characters one by one until encountering the newline character (`'\n'`)
1. **Severe Performance Inefficiency:**
- Each single-byte read triggers an **Operating System (OS) system call** to pull data from the OS buffer
- Reading a 1,000-character input requires 1,000 separate OS calls, creating massive overhead

## **5. The `Reader` Hierarchy and `BufferedReader`**

To overcome byte-level reading bottlenecks and character encoding complexities, Java introduced the abstract **Reader** **class** in `java.io`, which works directly with **streams of characters** rather than streams of byte

```
               Reader (Abstract Class)
              /           |           \
     BufferedReader  InputStreamReader  FileReader

```

### **`BufferedReader`**
- **Internal Memory Buffer:** Maintains an internal buffer in **program memory (Java heap)**].
- **Bulk Chunk Fetching:** Instead of requesting one character per OS call, it reads a large chunk of data in a single OS operation into its Java buffer. Subsequent character reads pull directly from fast application memory without OS context switches

### **`InputStreamReader`: The Bridge Class**
- **Incompatibility Problem:** `System.in` is an `InputStream` (**byte stream**), but `BufferedReader` requires a `Reader` (**character stream**
- **Adapter Pattern:** **InputStreamReader** acts as an adapter/bridge that converts a stream of bytes into a stream of characters.

### **Wiring `BufferedReader` and `InputStreamReader`**

```
// Step-by-step instantiation:
InputStreamReader isr = new InputStreamReader(System.in);
 // Byte stream -&gt; Character stream
BufferedReader br = new BufferedReader(isr);             
 // Character stream -&gt; Buffered character stream

// Standard inline pattern:
BufferedReader br = new BufferedReader(new InputStreamReader(System.in)); // [44]

```
### **Reading Lines**
- Calling **br.readLine()** reads an entire line of text as a `String

### **Limitations of `BufferedReader`**
- **String-Only Output:** `readLine()` can only return a `String`
- **Manual Parsing Required:** To read primitive data types (e.g., integers or floats), strings must be manually parsed (e.g., `Integer.parseInt(br.readLine())`).
- **Checked Exceptions:** Requires handling or propagating `IOException.

## **6. The Modern Alternative: `Scanner` Class**
Introduced in **Java 1.5**, the **Scanner** class was designed to simplify user input and eliminate verbose boilerplate].

### **Key Characteristics**
- **Package:** Resides in **java.util**, not `java.io. It is a utility class that acts as a high-level wrapper over sources like `InputStream`, `File`, or `String
- **No Checked Exceptions:** Does not force `throws IOException` for standard console reading
- **Constructor:** Takes an input source, typically standard input: `new Scanner(System.in)

### **Tokenization & Delimiters**
- `Scanner` breaks input into discrete pieces called **tokens** using delimiters
- By default, it uses **whitespace** (spaces, tabs, newlines) as the delimiter.
- Example: The string `"Hello I am Aditya"` is parsed into four separate tokens.

### **Common `Scanner` Methods**
- **next()** **:** Reads and returns the next single word/token (stops at whitespace
- **nextLine()** **:** Reads the complete line up until the Enter key / newline
- **Primitive Parsers:** Automatically reads and parses primitives without manual casting:
    - `nextInt()`
    - `nextDouble()`
    - `nextFloat()`
    - `nextLong()`
    - `nextBoolean()`
    - `nextByte()` / `nextShort()`
## **7. Comparison: `Scanner` vs. `BufferedReader`**

| Feature                  | `BufferedReader`                                                    | `Scanner`                                                         |
| ------------------------ | ------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Package**              | `java.io                                                            | `java.util                                                        |
| **Stream Type**          | Character stream (via `InputStreamReader`)                          | High-level utility / parser wrapper                               |
| **Data Returned**        | Only `String` (`readLine()`) or single characters                   | Multiple types (`String`, `int`, `double`, `boolean`)             |
| **Tokenization / Regex** | No tokenization or regex processing                                 | Performs tokenization and internal regex matching                 |
| **Speed & Performance**  | **Fast & highly optimized                                           | **Slower** due to parsing and tokenization overhead               |
| **Exception Handling**   | Throws checked `IOException`                                        | Does not throw checked `IOException` for console                  |
| **Best Used For**        | Competitive programming, high-throughput parsing, large input files | Standard console programs, beginner code, quick multi-type inputs |