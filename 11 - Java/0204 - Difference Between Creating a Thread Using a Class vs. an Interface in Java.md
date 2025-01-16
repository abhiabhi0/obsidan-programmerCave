

In Java, threads can be created primarily in two ways: by extending the `Thread` class or by implementing the `Runnable` interface. Each approach has its own advantages and disadvantages, which can affect how you design your multithreaded applications. Below is a detailed explanation of both methods, including diagrams and code snippets.

#### 1. Creating a Thread by Extending the Thread Class

When you create a thread by extending the `Thread` class, you are essentially creating a subclass of `Thread` and overriding its `run()` method.

**Advantages:**
- Simplicity: This method is straightforward and easy to understand.
- Direct access to thread methods: You can directly call thread methods like `start()`, `sleep()`, and `join()`.

**Disadvantages:**
- Inheritance limitation: Since Java does not support multiple inheritance, if your class extends `Thread`, it cannot extend any other class.
- Resource consumption: Each thread created this way has its own instance of the thread class.

**Example Code:**

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread " + Thread.currentThread().getName() + " is running.");
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread();
        MyThread thread2 = new MyThread();
        thread1.start(); // Start thread 1
        thread2.start(); // Start thread 2
    }
}
```

#### 2. Creating a Thread by Implementing the Runnable Interface

When you implement the `Runnable` interface, you define a class that implements the `run()` method. You then create a `Thread` object, passing an instance of your runnable class to it.

**Advantages:**
- Flexibility: The class can extend another class since it does not extend `Thread`.
- Reusability: The same instance of a runnable object can be shared among multiple threads.
- Better separation of concerns: The logic of the task (in `run()`) is separated from the thread management.

**Disadvantages:**
- Indirect access to thread methods: You need to create a separate `Thread` object to start the runnable task.
- Slightly more complex syntax compared to extending `Thread`.

**Example Code:**

```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread " + Thread.currentThread().getName() + " is running.");
    }
}

public class RunnableExample {
    public static void main(String[] args) {
        Thread thread1 = new Thread(new MyRunnable());
        Thread thread2 = new Thread(new MyRunnable());
        thread1.start(); // Start thread 1
        thread2.start(); // Start thread 2
    }
}
```

### Comparison Table

| Feature                      | Extending Thread Class                | Implementing Runnable Interface       |
|------------------------------|---------------------------------------|---------------------------------------|
| **Inheritance**              | Cannot extend another class           | Can extend another class              |
| **Object Creation**          | Creates unique instances for each thread | Can share the same runnable instance among multiple threads |
| **Access to Thread Methods** | Direct access to all thread methods   | Must create a separate Thread object   |
| **Design Flexibility**       | Less flexible due to single inheritance | More flexible; better design options   |
| **Memory Consumption**       | Higher, as each thread has its own instance | Lower, as multiple threads can share one runnable instance |

### Diagrammatic Representation

Here’s a simple diagram illustrating both approaches:

```
+---------------------+               +---------------------+
|   Extending Thread  |               | Implementing Runnable|
|                     |               |                     |
| +-----------------+ |               | +-----------------+ |
| | MyThread        |<---extends------>| | MyRunnable      | |
| +-----------------+ |               | +-----------------+ |
|         ^           |               |         ^           |
|         |           |               |         |           |
|         +-----------+               +---------+-----------+
|              Thread                      Thread Object
+---------------------+               +---------------------+
```

### Conclusion

Choosing between extending the `Thread` class and implementing the `Runnable` interface depends on your specific use case. If you need to inherit from another class or want to share resources among threads, implementing `Runnable` is the better choice. If simplicity and direct access to threading methods are your priorities, extending the `Thread` class may be suitable.

Understanding these differences will help you make informed decisions when designing multithreaded applications in Java and prepare you for related questions in interviews.

Citations:
[1] https://www.javatpoint.com/how-to-create-a-thread-in-java
[2] https://www.simplilearn.com/tutorials/java-tutorial/thread-in-java
[3] https://www.geeksforgeeks.org/differences-between-interface-and-class-in-java/
[4] https://www.linkedin.com/advice/1/what-benefits-drawbacks-using-runnable-interface
[5] https://www.youtube.com/watch?v=Z4KSgLpY0d8
[6] https://www.geeksforgeeks.org/implement-runnable-vs-extend-thread-in-java/