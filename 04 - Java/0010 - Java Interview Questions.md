## Beginner Topics

### 1. **What is Java?**
   - Java is a high-level, object-oriented programming language designed to be platform-independent through the use of bytecode.
### 2. **Why is Java a platform-independent language?**
   - Java achieves platform independence by compiling code into bytecode, which can run on any system with a Java Virtual Machine (JVM) installed.
### 3. **What are the features of Java?**
   - Key features include:
     - Object-oriented
     - Platform-independent
     - Robust and secure
     - Multithreaded
     - High performance

### 4. **What is JVM, JRE, and JDK?**
   - **JVM (Java Virtual Machine)**: Executes Java bytecode.
   - **JRE (Java Runtime Environment)**: Provides libraries and JVM to run Java applications.
   - **JDK (Java Development Kit)**: A full-featured software development kit for developing Java applications.

### 5. **What is the difference between a class and an object?**
   - A class is a blueprint for creating objects, while an object is an instance of a class that contains state (attributes) and behavior (methods).

### 6. **Collections in Java**
   - A collection is a framework that provides an architecture to store and manipulate a group of objects. The main interfaces include `Collection`, `List`, `Set`, and `Map`.

### 7. **Finding Even Numbers in a List**
   - Use Java 8 Streams to filter even numbers from a list.

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class EvenNumbers {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
        List<Integer> evenNumbers = numbers.stream()
                                            .filter(n -> n % 2 == 0)
                                            .collect(Collectors.toList());
        System.out.println(evenNumbers);
    }
}
```

### 8. **Anonymous Class in Java**
   - An anonymous class is a local class without a name used to instantiate classes or interfaces.
   - [[0201 - Anonymous Class]]

```java
interface Greeting {
    void greet();
}

public class Main {
    public static void main(String[] args) {
        Greeting greeting = new Greeting() {
            public void greet() {
                System.out.println("Hello!");
            }
        };
        greeting.greet();
    }
}
```

### 9. **Functional Interface in Java**
   - A functional interface is an interface with a single abstract method that can be used as the assignment target for a lambda expression.
   - [[0202 - Functional Interface in Java]]

```java
@FunctionalInterface
interface MyFunctionalInterface {
    void myMethod();
}
```

## Intermediate Topics

### 10. **What are the four principles of OOP?**
   - **Encapsulation**: Bundling data with methods.
   - **Inheritance**: Mechanism to create new classes using existing classes.
   - **Polymorphism**: Ability to present the same interface for different underlying forms.
   - **Abstraction**: Hiding complex implementation details.

### 11. **What is inheritance?**
   - Inheritance allows one class to inherit fields and methods from another class, promoting code reuse.

### 12. **What is encapsulation?**
   - Encapsulation restricts direct access to some of an object's components.

### 13. **What is polymorphism?**
   - Polymorphism allows methods to do different things based on the object it acts upon.

### 14. **How do you implement a Stack using an Array?**

```java
class Stack {
    private int maxSize;
    private int[] stackArray;
    private int top;

    public Stack(int size) {
        maxSize = size;
        stackArray = new int[maxSize];
        top = -1;
    }

    public void push(int value) {
        stackArray[++top] = value;
    }

    public int pop() {
        return stackArray[top--];
    }
}
```

### 15. **How do you reverse a linked list in Java?**

```java
class Node {
    int data;
    Node next;

    Node(int d) {
        data = d;
        next = null;
    }
}

Node reverse(Node head) {
    Node prev = null;
    Node current = head;
    Node next = null;

    while (current != null) {
        next = current.next; // Store next node
        current.next = prev; // Reverse current node's pointer
        prev = current;      // Move pointers one position ahead
        current = next;
    }
    return prev; // New head of the reversed list
}
```

### 16. **What is the difference between a HashSet and a TreeSet?**
   - A `HashSet` uses a hash table for storage, allowing for constant time complexity for basic operations like add, remove, contains. A `TreeSet` uses a red-black tree structure which maintains order but has logarithmic time complexity for these operations.

### 17. **What is exception handling in Java?**
   - Exception handling provides a way to handle runtime errors using `try`, `catch`, and `finally` blocks.

### 18. **How do you handle multiple exceptions in Java?**

```java
try {
    // Code that may throw exceptions
} catch (IOException | SQLException ex) {
    // Handle multiple exceptions
}
```

## Advanced Topics

### 19. **What is deadlock in Java? How can you prevent it?**
   - A deadlock occurs when two or more threads are blocked forever, each waiting on the other. To prevent deadlock:
     - Avoid nested locks.
     - Use timeouts when trying to acquire locks.
     - Ensure consistent ordering in acquiring locks.

### 20. **What is a thread in Java? Provide an example of how to create a thread.**
- [[0203 - Threads in Java]]
- [[0204 - Difference Between Creating a Thread Using a Class vs. an Interface in Java]]

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start(); // Start the thread
    }
}
```

### 21. **Garbage Collection in Java**
   - Garbage collection automatically manages memory by reclaiming memory occupied by objects that are no longer referenced.
   - [[0205 - Garbage Collection in Java]]

### 22. **How HashMap is Implemented Internally**
   - A HashMap uses an array of buckets to store key-value pairs, handling collisions through linked lists or trees.
   - [[0206 - Internal Implementation of HashMap in Java]]

### 23. **Immutable Classes**
   - An immutable class cannot be modified after its creation; this can be achieved by making fields final and not providing setters.
   - [[0207 - Immutable Classes in Java]]

```java
public final class ImmutableClass {
    private final int value;

    public ImmutableClass(int value) {
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}
```

### 24. **Singleton Pattern**
   - The Singleton pattern ensures that a class has only one instance and provides a global point of access to it.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```


### 26. **Inversion of Control (IoC)**
   - IoC is a design principle where control of object creation is transferred from application code to frameworks like Spring.
   - [[0208 - Inversion of Control (IoC) in Java]]

### 27. **Rate Limiting System Design**
   - A rate-limiting system controls how frequently an action can occur within a specified time frame.

### 28. **Print All Nodes Visible from Left Side of BST**
   - To print nodes visible from the left side of a Binary Search Tree (BST), perform level-order traversal and print the first node at each level.

```java
class Node {
    int data;
    Node left, right;

    Node(int item) {
        data = item;
        left = right = null;
    }
}

void leftView(Node root) {
    // Implementation goes here
}
```

### 29. **String vs StringBuilder vs StringBuffer**
   - Strings are immutable; StringBuilder is mutable and not synchronized; StringBuffer is mutable and synchronized.
   - [[0209 - String vs StringBuilder vs StringBuffer in Java]]

### 30. **HashMap vs HashSet Comparison Table**

| Feature      | HashMap                        | HashSet                         |
|--------------|--------------------------------|---------------------------------|
| Storage      | Key-Value pairs                | Unique values                   |
| Allow Nulls? | Yes, one null key              | Yes                             |
| Performance  | O(1) for put/get               | O(1) for add/remove             |


Citations:
[1] https://techaffinity.com/blog/oops-concepts-in-java/
[2] https://raygun.com/blog/oop-concepts-java/
[3] https://www.geeksforgeeks.org/object-oriented-programming-oops-concept-in-java/
[4] https://www.w3schools.com/java/java_oop.asp
[5] https://stackify.com/oops-concepts-in-java/
[6] https://www.javatpoint.com/java-oops-concepts
[7] https://www.youtube.com/watch?v=LarQG8Leqbo