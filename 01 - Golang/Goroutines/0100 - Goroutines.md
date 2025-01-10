#### 1. **What Are Goroutines?**

- **Definition**: A goroutine is a lightweight execution thread managed by the Go runtime. It allows functions to run concurrently with other goroutines, enabling efficient multitasking within a program.
- **Creation**: To create a goroutine, prefix a function call with the `go` keyword:
    
    ```go
    go myFunction()
    ```
    
    This initiates `myFunction` to run concurrently with the rest of the program.
- **Main Goroutine**: Every Go program starts with a single goroutine known as the main goroutine. If the main goroutine exits, all other goroutines are terminated as well.
- **Comparison with Threads**:
    - Not OS threads.
    - Not exactly green threads (managed by a language runtime).
    - Goroutines abstract coroutines, which are nonpreemptive subroutines (functions or closures).
- **Nonpreemptive Nature of Coroutines**:
    - Coroutines have suspension and reentry points.
    - Goroutines’ suspension and resumption are managed automatically by the Go runtime, making them seemingly preemptive but only when blocked.

#### 2. **Preemptive vs. Nonpreemptive Scheduling**

- **Preemptive Scheduling**:
    - The system can interrupt and suspend tasks (threads or processes) at any point, regardless of the task's state.
    - Common in modern operating systems where the scheduler decides when to switch contexts.
- **Nonpreemptive Scheduling**:
    - Tasks run until they voluntarily yield control.
    - Suspension points are defined explicitly within the task.

#### Are Goroutines Preemptive or Nonpreemptive?

- Goroutines are **a special class of coroutines**:
    - Go’s runtime **observes** when a goroutine is blocked (e.g., on I/O or synchronization).
    - The scheduler **automatically suspends and resumes** goroutines based on their state.
- **Goroutines are not fully preemptive** but are not purely nonpreemptive either:
    - Preemption occurs only at safe points where a goroutine blocks.
    - This hybrid model allows efficient scheduling without requiring explicit suspension points in user code.

#### 3. **Concurrency and Parallelism**

- **Concurrency**: Multiple tasks can progress at the same time.
- **Parallelism**: Multiple tasks run simultaneously on different cores.
- Goroutines enable concurrency but do not imply parallelism.

#### 4. **M:N Scheduler**

- **Definition**: Go uses an M:N scheduler.
    - Maps M green threads (managed by Go runtime) to N OS threads.
    - Goroutines are scheduled onto green threads.
    - Scheduler ensures efficient distribution and execution.
    - When one goroutine blocks, another is scheduled.

#### 5. **Fork-Join Concurrency Model**

- **Fork**: Splitting off a branch of execution (child) that runs concurrently with its parent.
- **Join**: A point where child and parent execution merge.
- Go’s `go` statement represents the fork operation.
- ![[{A1933F55-D743-44B3-96F7-A281DDE9ED60}.png]]

#### 6. **Example of a Goroutine Without a Join Point**

```go
sayHello := func() {
    fmt.Println("hello")
}
go sayHello()
// continue doing other things
```

- In this example:
    - `sayHello` runs in a new goroutine.
    - No guarantee that `sayHello` will complete before `main` exits.
    - Result: Likely, "hello" is never printed because `main` may exit first.

#### 7. **Why Sleep Does Not Solve the Problem**

- **Using `time.Sleep`** increases the chance that the goroutine runs before `main` exits.
- However, it introduces a race condition instead of guaranteeing execution.

#### 8. **Creating a Join Point with sync.WaitGroup**

- To synchronize goroutines and avoid race conditions, use `sync.WaitGroup`.

#### Corrected Example with Join Point

```go
var wg sync.WaitGroup

sayHello := func() {
    defer wg.Done()
    fmt.Println("hello")
}

wg.Add(1)
go sayHello()
wg.Wait() // Join point
```

- **Explanation**:
    - `wg.Add(1)`: Increments the counter.
    - `wg.Done()`: Decrements the counter when the goroutine finishes.
    - `wg.Wait()`: Blocks until all goroutines complete.
- **Output**: "hello"

#### 9. **Characteristics of Goroutines**

- **Lightweight**: Goroutines consume less memory and incur lower overhead than traditional threads, allowing thousands to run simultaneously.
- **Communication via Channels**: Channels provide safe communication between goroutines without locks, enabling bidirectional data transfer.
- **Concurrency Model**: Suitable for high-performance, I/O-bound tasks by utilizing goroutines for efficient multitasking.

### Summary

- **Goroutines** are Go’s lightweight concurrency primitives managed by the Go runtime.
- **Concurrency Model**: Go uses an M:N scheduler and a fork-join model.
- **Join Points** are necessary for deterministic behavior, implemented using synchronization primitives like `sync.WaitGroup`.
- Avoid relying on `time.Sleep` to manage goroutine lifetimes—instead, use structured synchronization techniques for correctness and safety.
[[0101 - Concurrency in Go]]

Ref : Concurrency in Go (p38-p40)

Citations:
[1] https://www.scaler.com/topics/golang/goroutines/
[2] https://www.educative.io/answers/what-is-a-goroutine
[3] https://dev.to/sanjaysinghrajpoot/what-are-goroutines-in-golang-19mh
[4] https://dev.to/jeffotoni/a-little-about-goroutines-in-go-2f0f
[5] https://www.programiz.com/golang/goroutines
[6] https://www.geeksforgeeks.org/goroutines-concurrency-in-golang/