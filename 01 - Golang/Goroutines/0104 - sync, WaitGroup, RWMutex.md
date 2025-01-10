### The `sync` Package in Go

The `sync` package in Go provides essential concurrency primitives useful for low-level memory access synchronization. These primitives are foundational for managing concurrent operations and ensuring data consistency in multi-threaded environments. Let's explore some of the primary components of the `sync` package.

#### WaitGroup

`WaitGroup` is a synchronization mechanism that waits for a collection of goroutines to finish execution. It's especially useful when the result of the goroutines is either not needed or can be collected through other means, such as channels. Here's how to use a `WaitGroup`:

```go
var wg sync.WaitGroup

wg.Add(1)
go func() {
    defer wg.Done()
    fmt.Println("1st goroutine sleeping...")
    time.Sleep(1 * time.Second)
}()

wg.Add(1)
go func() {
    defer wg.Done()
    fmt.Println("2nd goroutine sleeping...")
    time.Sleep(2 * time.Second)
}()

wg.Wait()
fmt.Println("All goroutines complete.")
```

- `Add(1)` increments the counter to indicate a new goroutine is starting.
- `Done()` decrements the counter, signaling the completion of a goroutine.
- `Wait()` blocks the main goroutine until the counter returns to zero, ensuring all added goroutines have finished.

#### Customary Usage of `WaitGroup`

It's typical to call `Add` closely before launching the goroutines it tracks. This prevents race conditions where the main goroutine might reach `Wait` before the other goroutines have started.

Example with multiple goroutines:

```go
hello := func(wg *sync.WaitGroup, id int) {
    defer wg.Done()
    fmt.Printf("Hello from %v!\n", id)
}

const numGreeters = 5
var wg sync.WaitGroup
wg.Add(numGreeters)

for i := 0; i < numGreeters; i++ {
    go hello(&wg, i+1)
}

wg.Wait()
```

#### Mutex and RWMutex

##### Mutex

`Mutex` (mutual exclusion) is a locking mechanism that prevents multiple goroutines from accessing a critical section of code simultaneously. Here's an example:

```go
var count int
var lock sync.Mutex

increment := func() {
    lock.Lock()
    defer lock.Unlock()
    count++
    fmt.Printf("Incrementing: %d\n", count)
}

decrement := func() {
    lock.Lock()
    defer lock.Unlock()
    count--
    fmt.Printf("Decrementing: %d\n", count)
}

var arithmetic sync.WaitGroup
for i := 0; i <= 5; i++ {
    arithmetic.Add(1)
    go func() {
        defer arithmetic.Done()
        increment()
    }()
}

for i := 0; i <= 5; i++ {
    arithmetic.Add(1)
    go func() {
        defer arithmetic.Done()
        decrement()
    }()
}

arithmetic.Wait()
fmt.Println("Arithmetic complete.")
```

- `Lock()` requests exclusive access to the critical section.
- `Unlock()` releases the lock, allowing other goroutines to enter the critical section.

##### RWMutex

`RWMutex` is similar to `Mutex` but provides more control. It allows multiple goroutines to read a resource simultaneously, as long as no goroutine is writing to it. This can reduce the contention in read-heavy scenarios.

Example of using `RWMutex`:

```go
producer := func(wg *sync.WaitGroup, l sync.Locker) {
    defer wg.Done()
    for i := 5; i > 0; i-- {
        l.Lock()
        l.Unlock()
        time.Sleep(1 * time.Second)
    }
}

observer := func(wg *sync.WaitGroup, l sync.Locker) {
    defer wg.Done()
    l.Lock()
    defer l.Unlock()
}

test := func(count int, mutex, rwMutex sync.Locker) time.Duration {
    var wg sync.WaitGroup
    wg.Add(count + 1)
    beginTestTime := time.Now()
    go producer(&wg, mutex)
    for i := count; i > 0; i-- {
        go observer(&wg, rwMutex)
    }
    wg.Wait()
    return time.Since(beginTestTime)
}

var m sync.RWMutex
tw := tabwriter.NewWriter(os.Stdout, 0, 1, 2, ' ', 0)
defer tw.Flush()

fmt.Fprintf(tw, "Readers\tRWMutex\tMutex\n")
for i := 0; i < 20; i++ {
    count := int(math.Pow(2, float64(i)))
    fmt.Fprintf(tw, "%d\t%v\t%v\n", count, test(count, &m, m.RLocker()), test(count, &m, &m))
}
```

- `RLocker()` returns a `Locker` interface that only allows read locking.
- This example demonstrates how `RWMutex` performs better than `Mutex` when there are many readers but few writers.

Using `RWMutex` over `Mutex` can significantly reduce the contention and improve performance, particularly when read operations dominate write operations.

Ref: Concurrency in Go(p47-p52)