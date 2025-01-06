## Basic Java Concepts

1. **What is Java?**
   - Java is a high-level, object-oriented programming language designed to be platform-independent through the use of bytecode.

2. **Why is Java a platform-independent language?**
   - Java achieves platform independence by compiling code into bytecode, which can run on any system with a Java Virtual Machine (JVM) installed[2].

3. **What are the features of Java?**
   - Key features include:
     - Object-oriented
     - Platform-independent
     - Robust and secure
     - Multithreaded
     - High performance

4. **What is JVM, JRE, and JDK?**
   - **JVM (Java Virtual Machine)**: Executes Java bytecode.
   - **JRE (Java Runtime Environment)**: Provides libraries and JVM to run Java applications.
   - **JDK (Java Development Kit)**: A full-featured software development kit for developing Java applications.

5. **What is the difference between a class and an object?**
   - A class is a blueprint for creating objects, while an object is an instance of a class that contains state (attributes) and behavior (methods).

## Object-Oriented Programming

6. **What are the four principles of OOP?**
   - **Encapsulation**: Bundling data with methods that operate on that data.
   - **Inheritance**: Mechanism to create new classes using existing classes.
   - **Polymorphism**: Ability to present the same interface for different underlying forms (data types).
   - **Abstraction**: Hiding complex implementation details and showing only essential features.

7. **What is inheritance?**
   - Inheritance allows one class to inherit fields and methods from another class, promoting code reuse.

8. **What is encapsulation?**
   - Encapsulation restricts direct access to some of an object's components and can prevent the accidental modification of data.

9. **What is polymorphism?**
   - Polymorphism allows methods to do different things based on the object it is acting upon, typically achieved through method overriding or overloading.

## Data Structures and Algorithms

10. **How do you implement a Stack using an Array?**

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

11. **How do you reverse a linked list in Java?**

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

12. **What is the difference between a HashSet and a TreeSet?**
   - A `HashSet` uses a hash table for storage, allowing for constant time complexity for basic operations like add, remove, contains. A `TreeSet`, on the other hand, uses a red-black tree structure, which maintains order but has logarithmic time complexity for these operations.

## Exception Handling

13. **What is exception handling in Java?**
   - Exception handling in Java provides a way to handle runtime errors using `try`, `catch`, and `finally` blocks.

14. **How do you handle multiple exceptions in Java?**

```java
try {
    // Code that may throw exceptions
} catch (IOException | SQLException ex) {
    // Handle multiple exceptions
}
```

## Advanced Topics

15. **What is a thread in Java? Provide an example of how to create a thread.**

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

16. **What is deadlock in Java? How can you prevent it?**
   - A deadlock occurs when two or more threads are blocked forever, each waiting on the other. To prevent deadlock:
     - Avoid nested locks.
     - Use a timeout when trying to acquire locks.
     - Ensure consistent ordering in acquiring locks.

This collection covers foundational knowledge as well as practical coding skills necessary for Java interviews. Make sure to practice coding problems regularly and understand the underlying concepts for each question.

Citations:
[1] https://www.guvi.in/blog/40-java-interview-questions-for-freshers/
[2] https://www.simplilearn.com/tutorials/java-tutorial/java-interview-questions
[3] https://www.geeksforgeeks.org/java-interview-questions/
[4] https://www.javatpoint.com/corejava-interview-questions
[5] https://www.interviewbit.com/java-interview-questions/
[6] https://www.datacamp.com/blog/java-interview-questions