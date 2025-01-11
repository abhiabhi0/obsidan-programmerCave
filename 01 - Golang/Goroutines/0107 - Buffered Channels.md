#### Introduction to Buffered Channels

- **Buffered Channels**: Channels in Go that are created with a specific capacity, allowing a certain number of elements to be stored in the channel before blocking occurs.
- **Declaration and Instantiation**:
    
    ```go
    var dataStream chan interface{}
    dataStream = make(chan interface{}, 4)
    ```
    
    - This example creates a buffered channel with a capacity of four. It can hold up to four elements before any write operations block.

#### Characteristics of Buffered Channels

- **Buffered vs Unbuffered Channels**:
    
    - **Buffered Channels** have a non-zero capacity and allow writes to proceed until the buffer is full.
    - **Unbuffered Channels** are effectively buffered channels with a capacity of zero, meaning they block immediately on writes if no corresponding read is waiting.
    
    ```go
    a := make(chan int)       // Unbuffered channel
    b := make(chan int, 0)    // Buffered channel with capacity 0 (unbuffered)
    ```
    
    - Both `a` and `b` are functionally equivalent as unbuffered channels.

#### Example: Buffered Channel with Capacity

- **Example of Writing to a Buffered Channel**:
    
    ```go
    c := make(chan rune, 4)
    c <- 'A'
    c <- 'B'
    c <- 'C'
    c <- 'D'
    ```
    
    - The channel `c` can hold four `rune` values. The first four writes proceed without blocking.
    - On the fifth write (`c <- 'E'`), the goroutine is blocked until a read occurs to free up space in the buffer.

#### Buffered Channel Behavior

- **FIFO Queue**: Buffered channels act as a First-In-First-Out (FIFO) queue, where reads occur in the order of writes.
- **Direct Passing**: If a buffered channel is empty but has a waiting reader, the value from a writer is passed directly to the reader, bypassing the buffer.
- **Performance Considerations**: Buffered channels are useful for optimizing performance when the number of writes is known and can be handled efficiently without immediate reads.

#### Example: Full Buffered Channel

- **Full Buffer and Blocking Example**:
    
    ```go
    c := make(chan rune, 4)
    c <- 'A'
    c <- 'B'
    c <- 'C'
    c <- 'D'
    <-c // Reading 'A' to free up space
    c <- 'E' // Now this write is unblocked
    ```
    
    - Initially, the channel is full after four writes. Reading one value unblocks further writes.

#### Example: Buffered Channel in a Complete Program

- **Code Example**:
    
    ```go
    var stdoutBuff bytes.Buffer
    defer stdoutBuff.WriteTo(os.Stdout)
    
    intStream := make(chan int, 4)
    go func() {
        defer close(intStream)
        defer fmt.Fprintln(&stdoutBuff, "Producer Done.")
        for i := 0; i < 5; i++ {
            fmt.Fprintf(&stdoutBuff, "Sending: %d\n", i)
            intStream <- i
        }
    }()
    
    for integer := range intStream {
        fmt.Fprintf(&stdoutBuff, "Received %v.\n", integer)
    }
    ```
    
    - This example demonstrates buffered channels in use. The producer goroutine sends five integers to `intStream`. The main goroutine reads and processes these values.
    - **Output Example**:
        
        ```
        Sending: 0
        Sending: 1
        Sending: 2
        Sending: 3
        Sending: 4
        Producer Done.
        Received 0.
        Received 1.
        Received 2.
        Received 3.
        Received 4.
        ```
        

#### Best Practices with Buffered Channels

- **Capacity Management**: It’s crucial to choose an appropriate buffer size based on the expected workload and performance requirements.
- **Avoid Premature Optimization**: Buffered channels should be used judiciously to avoid masking potential deadlocks or other concurrency issues.
- **Use Cases**: Ideal for scenarios where a producer can quickly send multiple items without immediate consumption, thereby improving throughput.

Ref: Concurrency in Go (p70-p74)