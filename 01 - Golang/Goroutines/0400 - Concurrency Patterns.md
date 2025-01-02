#### Overview
- **Concurrency vs. Parallelism**:
  - Concurrency is about dealing with multiple tasks at once, while parallelism is about executing multiple tasks simultaneously.
  - Go's concurrency model uses goroutines and channels to simplify concurrent programming.

#### Key Concepts
- **Goroutines**:
  - Lightweight threads managed by the Go runtime.
  - Created using the `go` keyword followed by a function call.
- **Channels**:
  - Provide a way for goroutines to communicate and synchronize.
  - Can be buffered or unbuffered, affecting how they block operations.

### Common Concurrency Patterns
1. **Worker Pool Pattern**:
   - **Purpose**: Manage a pool of workers to process tasks concurrently.
   - **Implementation**:
     - Create a fixed number of goroutines (workers).
     - Use a channel to distribute tasks among workers.
     - Each worker reads from the task channel and processes tasks independently.
     - [[Worker Pool Pattern Implementation]]

2. **Fan-Out/Fan-In Pattern**:
   - **Fan-Out**: Distributing work across multiple goroutines.
     - Multiple goroutines listen on the same input channel, processing data concurrently.
     - [[Worker Pool Pattern Implementation]]
   - **Fan-In**: Merging results from multiple goroutines into a single channel.
     - Create a single output channel and have each goroutine send results to it.
     - [[FanIn Pattern Implementation]]

3. **Pipeline Pattern**:
   - **Purpose**: Process data through a series of stages, where each stage is handled by a separate goroutine.
   - **Implementation**:
     - Each stage has its own channel for input and output.
     - Data flows through the pipeline from one stage to the next, allowing for concurrent processing at each stage.

4. **Select Statement**:
   - Allows waiting on multiple channel operations.
   - Useful for handling timeouts or multiple sources of input.
   - Example usage includes handling signals or managing multiple goroutines with different completion criteria.
   - [[0700 - select in Golang]]

5. **Context Package**:
   - Provides a way to manage cancellation signals and deadlines across goroutines.
   - Useful for controlling the lifecycle of goroutines, especially in HTTP servers and clients.
   - [[Context Package for Cancellation and Timeouts]]

### Advanced Concurrency Patterns

1. **Bounded Parallelism**:
   - Limits the number of concurrent operations based on available resources (e.g., limiting the number of active goroutines).
   - Helps prevent resource exhaustion and improves system stability.

2. **Tee Channel Pattern**:
   - Splits data from one channel into multiple channels for different consumers.
   - Useful for logging or monitoring data without altering the original data flow.

3. **Ring Buffer Channel**:
   - Implements a circular buffer for managing data flow between producers and consumers.
   - Helps handle bursty workloads efficiently by allowing temporary overflow.

4. **Subscription Pattern**:
   - Listens for events or updates from an external source (e.g., APIs).
   - Uses channels to deliver updates to subscribers at regular intervals.

### Practical Examples
- Explore practical implementations of these patterns in Go through repositories and examples available online, such as:
  - [Go Concurrency Patterns GitHub Repository](https://github.com/lotusirous/go-concurrency-patterns)
  - Various articles demonstrating specific patterns like fan-in, fan-out, and pipelines.

### Conclusion
- Mastering concurrency patterns in Go is essential for building robust, scalable applications that efficiently utilize system resources.
- Understanding these patterns enables developers to tackle common concurrency challenges effectively while writing clean and maintainable code.

### References for Further Reading
- [Understanding Concurrency Patterns in Go](https://hackernoon.com/understanding-concurrency-patterns-in-go)
- [5 Useful Concurrency Patterns in Go](https://blog.devgenius.io/5-useful-concurrency-patterns-in-golang-8dc90ad1ea61?gi=8be17fbfa6aa)
- [Advanced Concurrency Patterns](https://www.karanpratapsingh.com/courses/go/advanced-concurrency-patterns)

Citations:
[1] https://github.com/lotusirous/go-concurrency-patterns
[2] https://hackernoon.com/understanding-concurrency-patterns-in-go
[3] https://blog.devgenius.io/5-useful-concurrency-patterns-in-golang-8dc90ad1ea61?gi=8be17fbfa6aa
[4] https://www.oreilly.com/library/view/concurrency-in-go/9781491941294/ch04.html
[5] https://www.youtube.com/watch?v=f6kdp27TYZs
[6] https://www.karanpratapsingh.com/courses/go/advanced-concurrency-patterns