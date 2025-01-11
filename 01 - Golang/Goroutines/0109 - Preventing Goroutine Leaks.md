### Introduction

Goroutines are a powerful feature in Go that allows for easy and efficient concurrent execution. However, despite their lightweight nature, they are not garbage collected by the runtime. This means that goroutines, if not properly managed, can lead to resource leaks, affecting the performance and reliability of the application. Ensuring that goroutines are correctly terminated when they are no longer needed is crucial for maintaining a healthy Go program.

### Paths to Goroutine Termination

A goroutine can terminate through:

1. **Completion of its work**
2. **Encountering an unrecoverable error**
3. **Receiving a signal to stop working**

While the first two paths are inherent to the algorithm being implemented, the third path requires explicit management to avoid leaks.

### Example of a Goroutine Leak

```go
doWork := func(strings <-chan string) <-chan interface{} {
    completed := make(chan interface{})
    go func() {
        defer fmt.Println("doWork exited.")
        defer close(completed)
        for s := range strings {
            // Do something interesting
            fmt.Println(s)
        }
    }()
    return completed
}

doWork(nil)
fmt.Println("Done.")
```

In the above example, passing a `nil` channel to `doWork` results in the goroutine waiting indefinitely for input, leading to a leak.

### Solution: Using a `done` Channel

To mitigate goroutine leaks, a common convention is to use a `done` channel. This channel is passed to the child goroutine, allowing the parent to signal the child to stop.

#### Example with a `done` Channel

```go
doWork := func(
    done <-chan interface{},
    strings <-chan string,
) <-chan interface{} {
    terminated := make(chan interface{})
    go func() {
        defer fmt.Println("doWork exited.")
        defer close(terminated)
        for {
            select {
            case s := <-strings:
                fmt.Println(s)
            case <-done:
                return
            }
        }
    }()
    return terminated
}

done := make(chan interface{})
terminated := doWork(done, nil)

go func() {
    time.Sleep(1 * time.Second)
    fmt.Println("Canceling doWork goroutine...")
    close(done)
}()

<-terminated
fmt.Println("Done.")
```

### Managing Writing Goroutines

Goroutines that write to a channel can also leak if they block on sending data to a channel that is no longer being read from. The solution is similar: use a `done` channel to signal the writer to stop.

#### Example with a Writing Goroutine

```go
newRandStream := func(done <-chan interface{}) <-chan int {
    randStream := make(chan int)
    go func() {
        defer fmt.Println("newRandStream closure exited.")
        defer close(randStream)
        for {
            select {
            case randStream <- rand.Int():
            case <-done:
                return
            }
        }
    }()
    return randStream
}

done := make(chan interface{})
randStream := newRandStream(done)
fmt.Println("3 random ints:")
for i := 1; i <= 3; i++ {
    fmt.Printf("%d: %d\n", i, <-randStream)
}
close(done)

// Simulate ongoing work
time.Sleep(1 * time.Second)
```

### Conclusion

By adopting the convention of passing a `done` channel to every goroutine, we ensure that each goroutine can be properly terminated when no longer needed. This practice helps prevent resource leaks, ensuring the application remains efficient and reliable. Proper goroutine management is a fundamental aspect of writing robust concurrent programs in Go.