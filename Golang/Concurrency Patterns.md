- [Boring Design Pattern](#boring-design-pattern)
- [Worker Pool Pattern](#worker-pool-pattern)
- [Fan-In](#fan-in)
- [Fan-Out](#fan-out)

### Summary
#### Boring Design Pattern
- **Purpose**: Process multiple independent tasks concurrently.
- **Use Cases**: I/O operations, event handling.
- **Code Structure**:
    - Each task runs in its goroutine.
    - Channels handle task results.
#### Worker Pool Pattern
- **Purpose**: Efficiently manage concurrent task processing with limited workers.
- **Use Cases**: Web servers, data processing, resource management.
- **Key Benefits**:
    - Optimal resource usage.
    - Controlled concurrency.
- **Code Structure**:
    - Workers fetch tasks from a job queue and push results into a result queue.
#### Fan-In Pattern
- **Purpose**: Merge data from multiple input channels into one.
- **Use Cases**: Aggregating data streams, web scraping.
- **Code Structure**:
    - Multiple producers send data to separate channels.
    - A central channel merges data via goroutines.
#### Fan-Out Pattern
- **Purpose**: Distribute tasks to multiple worker goroutines for parallel processing.
- **Use Cases**: Load balancing, real-time event processing.
- **Code Structure**:
    - A task queue distributes work among workers, which process tasks concurrently.
    
---
### Boring Design Pattern

1. **Small and Independent Tasks**: The boring design pattern is well-suited for scenarios where you have several small and independent tasks that can be executed concurrently. Each task can be encapsulated within a separate goroutine.
2. **I/O-Bound Operations**: For applications that involve I/O-bound operations (e.g., reading and writing to files, making network requests), the boring design pattern can be beneficial. Concurrently executing I/O-bound tasks using goroutines and channels can improve overall throughput and performance.
3. **Event Handling**: When dealing with events or event-driven programming, the boring design pattern can be employed to handle multiple events concurrently. Each event handler can run in its own goroutine, responding to events as they occur.

```go
// Boring function that returns a channel to send boring messages  
func boring(msg string) <-chan string {  
	ch := make(chan string)  
	go func() {  
		defer close(ch) // Close the channel when the goroutine exits  
		for i := 1; i <= 5; i++ {  
		ch <- fmt.Sprintf("%s: %d", msg, i)  
		}  
	}()  
	return ch  
}  
  
func main() {  
	aliceCh := boring("Alice")  
	bobCh := boring("Bob")  
  
	// Receive messages concurrently  
	var wg sync.WaitGroup  
	wg.Add(2)  
  
	go func() {  
		for msg := range aliceCh {  
			fmt.Println(msg)  
		}  
		wg.Done()  
	}()  
  
	go func() {  
		for msg := range bobCh {  
			fmt.Println(msg)  
		}  
		wg.Done()  
	}()  
  
	wg.Wait()  
}
```

### Worker Pool Pattern
Worker Pool pattern is a concurrency design pattern used to manage a group of worker goroutines that process incoming tasks concurrently. It involves creating a pool of goroutines (workers) to process tasks from a shared queue (job queue) efficiently. By limiting the number of concurrently running goroutines, the Worker Pool pattern helps prevent resource contention and overloading the system, leading to improved performance and stability.

**Use Cases for Worker Pool:**

**Web Servers and Network Applications:** Worker Pools are ideal for handling incoming client requests concurrently. Each incoming request can be treated as a task, and worker goroutines process these tasks in parallel, ensuring fast and responsive server performance.

**Data Processing and Batch Jobs:** When dealing with large datasets or performing computationally intensive operations, a Worker Pool can divide the workload into smaller tasks. The worker goroutines process these tasks in parallel, significantly reducing the overall processing time.

**Resource Management:** In scenarios where resource-intensive operations, such as database connections or file I/O, need to be managed efficiently, a Worker Pool can be employed. A pool of pre-initialized resources is maintained, and worker goroutines use these resources as needed, avoiding the overhead of creating and destroying resources for each task.

**Why Do We Need Worker Pools?**

**Optimal Resource Utilization:** Worker Pools help maintain a balance between the number of concurrent tasks and available system resources. By limiting the number of workers, we ensure efficient resource utilization and prevent resource exhaustion.

**Improved Responsiveness:** For applications handling multiple concurrent tasks, using a Worker Pool ensures that tasks are processed promptly, leading to improved responsiveness and reduced waiting times.

**Controlled Concurrency:** A Worker Pool provides a straightforward mechanism to control the degree of concurrency in the application. By adjusting the number of workers in the pool, we can fine-tune the application’s performance to match the available resources.

**Stability and Scalability:** Worker Pools play a crucial role in creating stable and scalable applications. They prevent potential bottlenecks and help the application gracefully handle varying workloads.

```go
// Job represents the task to be executed by a worker
type Job struct {
	ID int
}

// WorkerPool represents a pool of worker goroutines
type WorkerPool struct {
	numWorkers int
	jobQueue   chan Job
	results    chan int
	wg         sync.WaitGroup
}

// NewWorkerPool creates a new worker pool with the specified number of workers
func NewWorkerPool(numWorkers, jobQueueSize int) *WorkerPool {
	return &WorkerPool{
		numWorkers: numWorkers,
		jobQueue:   make(chan Job, jobQueueSize),
		results:    make(chan int, jobQueueSize),
	}
}

// worker function to process jobs from the queue
func (wp *WorkerPool) worker(id int) {
	defer wp.wg.Done()
	for job := range wp.jobQueue {
		// Do the actual work here
		fmt.Printf("Worker %d started job %d\n", id, job.ID)
		time.Sleep(time.Second) // Simulating work
		fmt.Printf("Worker %d finished job %d\n", id, job.ID)
		wp.results <- job.ID
	}
}

// Start starts the worker pool and dispatches jobs to workers
func (wp *WorkerPool) Start() {
	for i := 1; i <= wp.numWorkers; i++ {
		wp.wg.Add(1)
		go wp.worker(i)
	}
}

// Wait waits for all workers to finish and closes the results channel
func (wp *WorkerPool) Wait() {
	wp.wg.Wait()
	close(wp.results)
}

// AddJob adds a job to the job queue
func (wp *WorkerPool) AddJob(job Job) {
	wp.jobQueue <- job
}

// CollectResults collects and prints results from the results channel
func (wp *WorkerPool) CollectResults() {
	for result := range wp.results {
		fmt.Printf("Result received for job %d\n", result)
	}
}

func main() {
	numWorkers := 3
	numJobs := 10

	workerPool := NewWorkerPool(numWorkers, numJobs)

	// Adding jobs to the Job Queue
	for i := 1; i <= numJobs; i++ {
		workerPool.AddJob(Job{ID: i})
	}

	close(workerPool.jobQueue)

	workerPool.Start()
	workerPool.Wait()
	workerPool.CollectResults()
}
```

### Fan-In
 “Fan-In” pattern is used in concurrent programming to consolidate and merge data from multiple input sources (channels) into a single output channel.

1. **Aggregating Data from Multiple Sources:**  
    — **Real-time Data Collection:** When you need to collect data from multiple sources, such as sensors, devices, or data streams, the Fan-In pattern helps you consolidate data efficiently.
2. **Handling Concurrent Producers:**  
    — Multiple Goroutines Producing Data: If you have multiple goroutines that produce data concurrently, the Fan-In pattern provides a structured approach to handle their output without the complexity of managing individual channels for each goroutine.

Real-World Use Cases:  
— **Financial Data Aggregation:** Combining data from various financial markets or stock exchanges for analysis.  
— **Web Scraping:** Collecting data from multiple websites or APIs concurrently and merging the results.  
— **Distributed Systems Monitoring:** Gathering data from different nodes in a distributed system and centralizing it for monitoring and analysis.

```go
func producer(id int) <-chan int {
	ch := make(chan int)
	go func() {
		defer close(ch)
		for i := 0; i < 3; i++ {
			ch <- id*10 + i
		}
	}()
	return ch
}

func fanIn(inputs ...<-chan int) <-chan int {
	var wg sync.WaitGroup
	output := make(chan int)

	// Function to copy values from a channel to the output channel
	copy := func(c <-chan int) {
		defer wg.Done()
		for val := range c {
			output <- val
		}
	}

	wg.Add(len(inputs))
	for _, ch := range inputs {
		go copy(ch)
	}

	go func() {
		wg.Wait()
		close(output)
	}()

	return output
}

func main() {
	// Step-1 Producer
	ch1 := producer(1)
	ch2 := producer(20)

	// Step-1 FanIn
	mergedOutputChannel := fanIn(ch1, ch2)

	// Step-3 Consumer
	for val := range mergedOutputChannel {
		fmt.Println(val)
	}
}
```

### Fan-Out
Fan-Out pattern is a concurrency gem that allows us to efficiently distribute tasks among multiple worker goroutines for blazing-fast parallel processing. Whether you’re handling data, web scraping, load balancing, or real-time event processing, the Fan-Out pattern has your back.

```go
func source(tasks chan<- int, numTasks int) {
	for i := 1; i <= numTasks; i++ {
		tasks <- i
	}
	close(tasks)
}

func worker(id int, taskQueue <-chan int, results chan<- int, wg *sync.WaitGroup) {
	defer wg.Done()
	for task := range taskQueue {
		// Simulating task processing
		fmt.Printf("Worker %d processing task %d\n", id, task)
		time.Sleep(time.Millisecond * time.Duration(task))
		results <- task * 2 // task 1 * 2 | task 2 * 2
	}
}

func main() {
	const numWorkers = 3
	const numTasks = 10

	taskQueue := make(chan int, numTasks)
	resultQueue := make(chan int, numTasks)
	var wg sync.WaitGroup

	// Start the source goroutine to generate tasks
	go source(taskQueue, numTasks)

	// Create worker goroutines
	for i := 1; i <= numWorkers; i++ {
		wg.Add(1)
		go worker(i, taskQueue, resultQueue, &wg)
	}

	// Wait for all workers to finish
	go func() {
		wg.Wait()
		close(resultQueue)
	}()

	// Process results from the result queue
	for result := range resultQueue {
		fmt.Printf("Result received: %d\n", result)
	}

	fmt.Println("All tasks completed.")
}

```
