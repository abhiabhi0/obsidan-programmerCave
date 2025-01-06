In Java, `String`, `StringBuilder`, and `StringBuffer` are three classes used to handle sequences of characters. Each class has its own characteristics, advantages, and use cases. Understanding these differences is crucial for writing efficient and effective code.

#### 1. String

- **Immutability**: The `String` class is immutable, meaning that once a `String` object is created, its value cannot be changed. Any modification to a string results in the creation of a new `String` object.
  
- **Storage**: Strings are stored in a special area of memory called the **String Pool** (or String Intern Pool). This pool helps optimize memory usage by storing only one copy of each distinct string literal.

- **Performance**: Due to immutability, frequent modifications (like concatenation) can lead to performance issues because new objects are created each time.

- **Example**:
```java
String str = "Java";
str += " Guides"; // A new String object is created
```

#### 2. StringBuilder

- **Mutability**: The `StringBuilder` class is mutable, allowing modifications to the character sequence without creating new objects. This makes it more efficient for frequent changes.

- **Thread Safety**: `StringBuilder` is not synchronized, making it non-thread-safe. It is faster than `StringBuffer` due to the lack of synchronization overhead.

- **Usage**: Ideal for use in single-threaded scenarios where performance is critical and frequent modifications are expected.

- **Example**:
```java
StringBuilder sb = new StringBuilder("Java");
sb.append(" Guides"); // Modifies the same object
```

#### 3. StringBuffer

- **Mutability**: Like `StringBuilder`, the `StringBuffer` class is also mutable, allowing modifications without creating new objects.

- **Thread Safety**: `StringBuffer` is synchronized, making it thread-safe. This means multiple threads can safely use it at the same time, but this comes at the cost of performance due to synchronization overhead.

- **Usage**: Best suited for multi-threaded environments where thread safety is required during string manipulations.

- **Example**:
```java
StringBuffer sbf = new StringBuffer("Java");
sbf.append(" Guides"); // Modifies the same object safely in a thread-safe manner
```

### Comparison Summary

| Feature                | String                         | StringBuilder                | StringBuffer                |
|------------------------|--------------------------------|------------------------------|-----------------------------|
| **Immutability**       | Immutable                      | Mutable                      | Mutable                     |
| **Thread Safety**      | Thread-safe (inherently)      | Not thread-safe              | Thread-safe (synchronized)  |
| **Performance**        | Slower with frequent changes   | Faster for modifications     | Slower due to synchronization|
| **Storage Area**       | Stored in String Pool          | Stored in heap               | Stored in heap              |
| **Concatenation**      | Creates new objects            | Efficient append method      | Efficient append method      |
| **Use Case**           | When text won't change         | Single-threaded applications | Multi-threaded applications  |

### String Pool

The **String Pool** is a special memory region used by the Java Virtual Machine (JVM) to store string literals. When a string literal is created, the JVM checks if an identical string already exists in the pool:

- If it exists, the reference to the existing string is returned.
- If it does not exist, a new string object is created and added to the pool.

This mechanism helps save memory and improves performance when strings are reused.

### Example Illustrating String Pool

```java
public class Main {
    public static void main(String[] args) {
        String str1 = "Hello"; // Created in String Pool
        String str2 = "Hello"; // Reuses reference from String Pool
        
        System.out.println(str1 == str2); // Output: true
        
        // Creating a new string using 'new' keyword
        String str3 = new String("Hello"); // Created in heap memory, not in pool
        
        System.out.println(str1 == str3); // Output: false
    }
}
```

### Conclusion

Choosing between `String`, `StringBuilder`, and `StringBuffer` depends on your specific requirements regarding mutability, thread safety, and performance:

- Use **`String`** when you need an immutable sequence of characters.
- Use **`StringBuilder`** when you need a mutable sequence of characters in a single-threaded environment for better performance.
- Use **`StringBuffer`** when you need a mutable sequence of characters that will be accessed by multiple threads safely.

Understanding these distinctions will help you write more efficient and maintainable Java code.

Citations:
[1] https://www.javaguides.net/2023/08/string-vs-stringbuilder-vs-stringbuffer.html
[2] https://byjus.com/gate/difference-between-stringbuffer-and-stringbuilder/
[3] https://www.geeksforgeeks.org/stringbuffer-vs-stringbuilder/
[4] https://www.shiksha.com/online-courses/articles/stringbuilder-vs-string-buffer/