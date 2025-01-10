#### `Cond`

- **Definition**: `Cond` is used as a rendezvous point for goroutines waiting for or announcing the occurrence of an event. An event is an arbitrary signal with no information other than its occurrence.
    
- **Example without `Cond`**:
    
    ```go
    for conditionTrue() == false {
        time.Sleep(1 * time.Millisecond)
    }
    ```
    
    - This loop is inefficient, either sleeping too long or consuming too much CPU.
- **Using `Cond`**:
    
    ```go
    c := sync.NewCond(&sync.Mutex{})
    c.L.Lock()
    for conditionTrue() == false {
        c.Wait()
    }
    c.L.Unlock()
    ```
    
    - `Wait()` suspends the goroutine, releasing the lock until it is signaled to wake up.
- **Queue Example**:
    
    ```go
    c := sync.NewCond(&sync.Mutex{})
    queue := make([]interface{}, 0, 10)
    removeFromQueue := func(delay time.Duration) {
        time.Sleep(delay)
        c.L.Lock()
        queue = queue[1:]
        fmt.Println("Removed from queue")
        c.L.Unlock()
        c.Signal()
    }
    
    for i := 0; i < 10; i++ {
        c.L.Lock()
        for len(queue) == 2 {
            c.Wait()
        }
        fmt.Println("Adding to queue")
        queue = append(queue, struct{}{})
        go removeFromQueue(1 * time.Second)
        c.L.Unlock()
    }
    ```
    
    - Demonstrates the efficient handling of queue operations using `Cond`.
- **Broadcasting Signals**:
    
    ```go
    type Button struct {
        Clicked *sync.Cond
    }
    button := Button{Clicked: sync.NewCond(&sync.Mutex{})}
    subscribe := func(c *sync.Cond, fn func()) {
        go func() {
            c.L.Lock()
            defer c.L.Unlock()
            c.Wait()
            fn()
        }()
    }
    subscribe(button.Clicked, func() { fmt.Println("Maximizing window.") })
    subscribe(button.Clicked, func() { fmt.Println("Displaying dialog.") })
    button.Clicked.Broadcast()
    ```
    
    - `Broadcast()` notifies all goroutines waiting on a condition.

#### `Once`

- **Purpose**: Ensures a function is only executed once, even when called from multiple goroutines.
    
- **Example**:
    
    ```go
    var once sync.Once
    var count int
    increment := func() { count++ }
    
    var wg sync.WaitGroup
    wg.Add(100)
    for i := 0; i < 100; i++ {
        go func() {
            defer wg.Done()
            once.Do(increment)
        }()
    }
    wg.Wait()
    fmt.Printf("Count is %d\n", count)
    ```
    
    - Output: `Count is 1`
    - `sync.Once` guarantees the function inside `Do` is only called once.
- **Deadlock Example**:
    
    ```go
    var onceA, onceB sync.Once
    var initB func()
    initA := func() { onceB.Do(initB) }
    initB = func() { onceA.Do(initA) }
    onceA.Do(initA)
    ```
    
    - Circular references can cause deadlocks.

#### `Pool`

- **Purpose**: Implements the object pool pattern for managing reusable objects efficiently.
    
- **Basic Usage**:
    
    ```go
    myPool := &sync.Pool{
        New: func() interface{} {
            fmt.Println("Creating new instance.")
            return struct{}{}
        },
    }
    
    myPool.Get() // Creates new instance
    instance := myPool.Get()
    myPool.Put(instance) // Reuses instance
    myPool.Get()
    ```
    
- **Memory Efficiency**:
    
    ```go
    var numCalcsCreated int
    calcPool := &sync.Pool{
        New: func() interface{} {
            numCalcsCreated++
            return make([]byte, 1024)
        },
    }
    
    calcPool.Put(calcPool.New())
    const numWorkers = 1024 * 1024
    var wg sync.WaitGroup
    wg.Add(numWorkers)
    
    for i := numWorkers; i > 0; i-- {
        go func() {
            defer wg.Done()
            mem := calcPool.Get().(*[]byte)
            defer calcPool.Put(mem)
        }()
    }
    wg.Wait()
    fmt.Printf("%d calculators were created.", numCalcsCreated)
    ```
    
    - Demonstrates memory optimization by reusing objects.
- **Performance Enhancement**:
    
    ```go
    func connectToService() interface{} {
        time.Sleep(1 * time.Second)
        return struct{}{}
    }
    
    func startNetworkDaemon() *sync.WaitGroup {
        var wg sync.WaitGroup
        wg.Add(1)
        go func() {
            server, _ := net.Listen("tcp", "localhost:8080")
            defer server.Close()
            wg.Done()
            for {
                conn, _ := server.Accept()
                connectToService()
                fmt.Fprintln(conn, "")
                conn.Close()
            }
        }()
        return &wg
    }
    ```
    
    - Using `sync.Pool` for connection reuse can significantly reduce the time per operation.

---

These notes provide a comprehensive summary of the key points from the text, with practical examples and explanations for each concept. Let me know if you need further details or additional notes on any specific part.