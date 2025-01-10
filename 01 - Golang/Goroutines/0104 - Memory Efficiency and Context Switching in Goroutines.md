### Goroutines: Lightweight and Efficient

Goroutines are an extraordinarily lightweight alternative to traditional threads, designed to optimize concurrent programming in Go. According to the Go [FAQ](https://go.dev/doc/faq#goroutines):

- A newly created goroutine starts with only a few kilobytes of stack memory.
- The Go runtime automatically grows and shrinks the memory allocated to a goroutine’s stack as needed.
- CPU overhead is minimal, averaging about three instructions per function call.
- This lightweight design enables the creation of hundreds of thousands of goroutines within a single address space without exhausting system resources.

**Garbage Collection of Goroutines**

- Unlike regular objects, goroutines are not automatically garbage collected when abandoned.
- Example:
    
    ```go
    go func() {
      // operation that will block forever
    }()
    ```
    
    - This goroutine will persist until the entire process terminates, contributing to resource leaks if not handled properly.

### Measuring the Memory Overhead of Goroutines

We can measure the memory consumption of a goroutine by introspecting the runtime’s memory stats before and after creating them:

```go
memConsumed := func() uint64 {
    runtime.GC()
    var stats runtime.MemStats
    runtime.ReadMemStats(&stats)
    return stats.Sys
}

var c <-chan interface{}
var wg sync.WaitGroup
noop := func() { wg.Done(); <-c }

const numGoroutines = 1e4
wg.Add(numGoroutines)

before := memConsumed()
for i := numGoroutines; i > 0; i-- {
    go noop()
}

wg.Wait()
after := memConsumed()

fmt.Printf("%.3fkb", float64(after-before)/numGoroutines/1000)
```

#### Explanation

- **Persistent Goroutine**: We create goroutines that wait forever to keep them in memory for measurement.
- **Law of Large Numbers**: We create a large number (`1e4`) of goroutines for a meaningful estimate.
- **Memory Measurement**: We compute memory usage before and after creating goroutines.

#### Sample Output

- Memory consumed per goroutine: **2.817 KB**

#### Scalability Estimates

|Memory (GB)|Approximate Goroutines|
|---|---|
|1|~3,718,000|
|8|~29,746,000|
|128|~951,867,000|

### Context Switching: Threads vs. Goroutines

Context switching occurs when the system saves the state of one process and restores another. Goroutines leverage a more efficient, software-based scheduler compared to OS threads:

#### Benchmark: OS Thread Context Switch (Linux)

```bash
taskset -c 0 perf bench sched pipe -T
```

- Time per operation: 2.935 microseconds
- Context switch cost (half operation): **1.467 microseconds**

#### Benchmark: Goroutine Context Switch

```go
func BenchmarkContextSwitch(b *testing.B) {
    var wg sync.WaitGroup
    begin := make(chan struct{})
    c := make(chan struct{})
    var token struct{}

    sender := func() {
        defer wg.Done()
        <-begin
        for i := 0; i < b.N; i++ {
            c <- token
        }
    }

    receiver := func() {
        defer wg.Done()
        <-begin
        for i := 0; i < b.N; i++ {
            <-c
        }
    }

    wg.Add(2)
    go sender()
    go receiver()
    b.StartTimer()
    close(begin)
    wg.Wait()
}
```

- Measured context switch cost: **225 nanoseconds**
- **92% faster** than OS thread switching.

### Conclusion

- Goroutines are highly scalable, efficient, and cost-effective for concurrent programming.
- While context switching between goroutines is vastly cheaper than OS threads, excessive goroutines can still lead to performance issues if not carefully managed.
- Always profile performance before optimizing goroutine usage in your application.

By understanding these characteristics, Go developers can confidently leverage goroutines for building scalable and performant applications.

Ref: Concurrency in Go (p43-p47)