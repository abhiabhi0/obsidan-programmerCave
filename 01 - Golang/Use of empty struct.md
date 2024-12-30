## As a semaphore or lock

Because the empty struct has no fields, it can be conveniently used to implement some concurrency control functions, such as mutex locks, read-write locks. We can use `chan struct{}` to implement an unbuffered channel for controlling concurrent access.

```go
func main() {
    var wg sync.WaitGroup
    var mu sync.Mutex
    c := make(chan struct{}, 1)

    for i := 0; i < 10; i++ {
        wg.Add(1)
        go func(i int) {
            c <- struct{}{} // acquire lock
            defer func() {
                <-c // release lock
                wg.Done()
            }()

            mu.Lock()
            defer mu.Unlock()
            fmt.Printf("goroutine %d: hello\n", i)
        }(i)
    }

    wg.Wait()
}
```

In the above code, we use an empty struct `struct{}` to represent a semaphore. When a goroutine acquires the lock, it sends an empty struct to the channel to indicate that it has acquired the lock. After executing, it receives an empty struct from the channel to release the lock.