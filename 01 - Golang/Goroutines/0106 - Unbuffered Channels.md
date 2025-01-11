```go
var dataStream chan interface{} 
dataStream = make(chan interface{})
```

- Here we declare a channel. We say it is “of type” `interface{}` since the type we’ve declared is the empty interface.
- Here we instantiate the channel using the built-in make function.

- To declare a unidirectional channel, you’ll simply include the `<-` operator. To both declare and instantiate a channel that can only read, place the `<-` operator on the lefthand side, like so:
```go
var dataStream <-chan interface{} 
dataStream := make(<-chan interface{})
```

- And to declare and create a channel that can only send, you place the `<-` operator on the righthand side, like so:
```go
var dataStream chan<- interface{} 
dataStream := make(chan<- interface{})
```
#### Goroutines Scheduling and Blocking

- **Goroutine Scheduling**: A scheduled goroutine does not guarantee execution before the process exits. The Go runtime scheduler manages goroutines and their execution based on availability and resource usage.
- **Blocking Nature of Channels**: Channels in Go are designed to block until an operation can proceed, ensuring synchronization between goroutines.
    - **Write Blocking**: When a goroutine tries to write to a full channel, it blocks until space becomes available, allowing synchronization between producer and consumer goroutines.
    - **Read Blocking**: When a goroutine tries to read from an empty channel, it blocks until a value is available, ensuring that the consumer waits for data from the producer.

#### Example 1: Basic Channel Blocking

```go
stringStream := make(chan string)
go func() {
    stringStream <- "Hello channels!"
}()
fmt.Println(<-stringStream)
```

- In this example, the anonymous goroutine writes to `stringStream` and blocks until the value is read by the main goroutine. This guarantees that the main goroutine will not proceed to print until the value is available, ensuring synchronization.

#### Example 2: Deadlock Scenario

```go
stringStream := make(chan string)
go func() {
    if 0 != 1 {
        return
    }
    stringStream <- "Hello channels!"
}()
fmt.Println(<-stringStream)
```

- The conditional statement prevents any value from being written to `stringStream`, causing the main goroutine to block indefinitely while waiting for a value. This results in a deadlock.
    - **Output**: `fatal error: all goroutines are asleep - deadlock!`

#### Handling Deadlocks

- To prevent deadlocks, it is essential to structure programs to ensure that both write and read operations on channels can proceed as expected. Proper channel usage patterns and ensuring that goroutines can complete their intended tasks are critical.

#### Receiving Two Values from a Channel

```go
stringStream := make(chan string)
go func() {
    stringStream <- "Hello channels!"
}()
salutation, ok := <-stringStream
fmt.Printf("(%v): %v", ok, salutation)
```

- The boolean `ok` indicates whether the value read was from a successful write (`true`) or if the channel is closed (`false`).
    - **Output**: `(true): Hello channels!`

#### Closing Channels

- **Closing a Channel**: Using `close(channelName)` signals that no more values will be sent on the channel, allowing downstream goroutines to know when to stop waiting for new data.
    - **Reading from a Closed Channel**: Reading from a closed channel returns the zero value of the channel's type and `false`.

#### Example 3: Reading from a Closed Channel

```go
intStream := make(chan int)
close(intStream)
integer, ok := <-intStream
fmt.Printf("(%v): %v", ok, integer)
```

- Since the channel is closed immediately without any value being sent, reading from it returns the zero value for `int` (which is `0`) and `false` for `ok`.
    - **Output**: `(false): 0`

#### Using `range` with Channels

```go
intStream := make(chan int)
go func() {
    defer close(intStream)
    for i := 1; i <= 5; i++ {
        intStream <- i
    }
}()
for integer := range intStream {
    fmt.Printf("%v ", integer)
}
```

- The `range` keyword iterates over the channel and automatically stops when the channel is closed, simplifying the loop structure.
    - **Output**: `1 2 3 4 5`

#### Unblocking Multiple Goroutines

```go
begin := make(chan interface{})
var wg sync.WaitGroup

for i := 0; i < 5; i++ {
    wg.Add(1)
    go func(i int) {
        defer wg.Done()
        <-begin
        fmt.Printf("%v has begun\n", i)
    }(i)
}
fmt.Println("Unblocking goroutines...")
close(begin)
wg.Wait()
```

- By closing the `begin` channel, all waiting goroutines are unblocked simultaneously. This pattern is efficient for signaling multiple goroutines to proceed.
    
    - **Output**:
    
    ```
    Unblocking goroutines...
    4 has begun
    2 has begun
    3 has begun
    0 has begun
    1 has begun
    ```
    
- This approach is more efficient than writing to the channel multiple times to unblock each goroutine individually.

Ref: Concurrency in Go (p67-p70)