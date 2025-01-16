Threads are a fundamental concept in Java that allows concurrent execution of two or more parts of a program. This capability enables Java applications to perform multiple operations simultaneously, improving performance and responsiveness.

#### Key Concepts

1. **What is a Thread?**
   - A thread is a lightweight process, the smallest unit of processing that can be scheduled by the operating system. Java supports multithreading, allowing multiple threads to run concurrently.

2. **Multithreading**
   - Multithreading is the concurrent execution of two or more threads. It helps in resource sharing and improves the performance of applications, especially in scenarios where tasks can be performed independently.

3. **Creating Threads**
   - In Java, threads can be created in two primary ways:
     - **Extending the `Thread` class**: By creating a subclass of `Thread` and overriding its `run()` method.
     - **Implementing the `Runnable` interface**: By implementing the `Runnable` interface and defining its `run()` method.
     - [[0204 - Difference Between Creating a Thread Using a Class vs. an Interface in Java]]

### Creating Threads

#### 1. Extending the `Thread` Class

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread " + Thread.currentThread().getName() + " is running.");
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread();
        MyThread thread2 = new MyThread();
        thread1.start(); // Start thread 1
        thread2.start(); // Start thread 2
    }
}
```

#### 2. Implementing the `Runnable` Interface

```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread " + Thread.currentThread().getName() + " is running.");
    }
}

public class RunnableDemo {
    public static void main(String[] args) {
        Thread thread1 = new Thread(new MyRunnable());
        Thread thread2 = new Thread(new MyRunnable());
        thread1.start(); // Start thread 1
        thread2.start(); // Start thread 2
    }
}
```

### Thread Life Cycle

The life cycle of a thread consists of several states, which are represented in the following diagram:

```
+-------------------+
|      New          |  --> Created but not yet started
+-------------------+
          |
          v
+-------------------+
|      Runnable      |  --> Ready to run or currently running
+-------------------+
          |
          v
+-------------------+
|      Blocked       |  --> Waiting for a monitor lock
+-------------------+
          |
          v
+-------------------+
|      Waiting       |  --> Waiting indefinitely for another thread to perform a particular action
+-------------------+
          |
          v
+-------------------+
|      Timed Waiting |  --> Waiting for another thread to perform an action for a specified waiting time
+-------------------+
          |
          v
+-------------------+
|      Terminated    |  --> Completed execution
+-------------------+
```

### Thread States Explained

1. **New**: The thread is created but not yet started.
2. **Runnable**: The thread is ready to run and waiting for CPU time.
3. **Blocked**: The thread is blocked waiting for a monitor lock (for synchronized code).
4. **Waiting**: The thread is waiting indefinitely for another thread to perform an action (e.g., `wait()`).
5. **Timed Waiting**: The thread is waiting for another thread to perform an action for a specified period (e.g., `sleep()`).
6. **Terminated**: The thread has completed execution.

### Thread Methods

Java provides several methods in the `Thread` class to manage threads:

- **start()**: Starts the execution of the thread.
- **run()**: Contains the code that defines what the thread will do.
- **sleep(milliseconds)**: Causes the currently executing thread to sleep for a specified number of milliseconds.
- **join()**: Waits for a thread to die.
- **interrupt()**: Interrupts a sleeping or waiting thread.
- **setPriority(int priority)**: Changes the priority of a thread.

### Example of Using Thread Methods

```java
class SleepDemo extends Thread {
    public void run() {
        try {
            System.out.println(Thread.currentThread().getName() + " is sleeping.");
            Thread.sleep(2000); // Sleep for 2 seconds
            System.out.println(Thread.currentThread().getName() + " woke up.");
        } catch (InterruptedException e) {
            System.out.println(Thread.currentThread().getName() + " was interrupted.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        SleepDemo t1 = new SleepDemo();
        t1.start();
    }
}
```

### Synchronization

Synchronization is essential in multithreaded programming to prevent data inconsistency when multiple threads access shared resources. Java provides synchronized methods and blocks to control access.

```java
class Counter {
    private int count = 0;

    public synchronized void increment() { // Synchronized method
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class SyncDemo extends Thread {
    Counter counter;

    SyncDemo(Counter counter) {
        this.counter = counter;
    }

    public void run() {
        for (int i = 0; i < 1000; i++) {
            counter.increment();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        SyncDemo t1 = new SyncDemo(counter);
        SyncDemo t2 = new SyncDemo(counter);
        
        t1.start();
        t2.start();
        
        t1.join();
        t2.join();
        
        System.out.println("Final Count: " + counter.getCount());
    }
}
```

### Conclusion

Understanding threads in Java is crucial for developing efficient and responsive applications. This includes knowing how to create threads, manage their life cycle, synchronize access to shared resources, and utilize various methods provided by the `Thread` class.

These notes provide a comprehensive overview of threads in Java, including examples and diagrams that should help you prepare effectively for interviews on this topic.

Citations:
[1] https://www.javabrahman.com/corejava/understanding-thread-life-cycle-thread-states-in-java-tutorial-with-examples/
[2] https://www.tutorialspoint.com/java/java_multithreading.htm
[3] https://www.geeksforgeeks.org/java-multithreading-tutorial/
[4] https://www.scientecheasy.com/2020/08/java-thread-tutorial.html/
[5] https://jenkov.com/tutorials/java-concurrency/java-virtual-threads.html