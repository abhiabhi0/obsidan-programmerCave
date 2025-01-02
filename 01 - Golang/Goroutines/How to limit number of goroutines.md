To limit the number of goroutines in Go, you can use various methods, including buffered channels, worker pools, or libraries designed for this purpose. Below are some examples and explanations of how to implement these techniques effectively.

### 1. Using Buffered Channels

One of the simplest ways to limit the number of concurrent goroutines is to use a buffered channel as a semaphore. When the channel is full, any attempt to send a new goroutine will block until a slot becomes available.

#### Example Code

```go
package main

import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

const MAX_CONCURRENT_JOBS = 2 // Limit to 2 concurrent jobs

func main() {
    waitChan := make(chan struct{}, MAX_CONCURRENT_JOBS) // Buffered channel to limit goroutines
    var wg sync.WaitGroup

    for i := 1; i <= 10; i++ {
        waitChan <- struct{}{} // Block if the channel is full
        wg.Add(1)

        go func(index int) {
            defer wg.Done()
            job(index)
            <-waitChan // Release a slot in the channel
        }(i)
    }

    wg.Wait() // Wait for all goroutines to finish
}

func job(index int) {
    fmt.Printf("Job %d: starting\n", index)
    time.Sleep(time.Duration(rand.Intn(3)) * time.Second) // Simulate work
    fmt.Printf("Job %d: done\n", index)
}
```

### Explanation

- **Buffered Channel**: The `waitChan` channel is created with a buffer size equal to `MAX_CONCURRENT_JOBS`. This limits the number of concurrently running goroutines.
- **Blocking Behavior**: When the buffer is full, trying to send a new value into `waitChan` will block until a slot becomes available (i.e., when one of the goroutines finishes).
- **Goroutine Management**: Each goroutine performs its job and signals completion by reading from `waitChan`, allowing another goroutine to start.

### 2. Using Worker Pool Pattern

Another effective way to limit the number of concurrent goroutines is by implementing a worker pool. This pattern allows you to create a fixed number of worker goroutines that process jobs from a shared job queue.

#### Example Code

```go
package main

import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

const (
    NUM_WORKERS = 3 // Number of workers
    NUM_JOBS    = 10 // Total jobs
)

func worker(id int, jobs <-chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    for job := range jobs {
        fmt.Printf("Worker %d: processing job %d\n", id, job)
        time.Sleep(time.Duration(rand.Intn(3)) * time.Second) // Simulate work
        fmt.Printf("Worker %d: completed job %d\n", id, job)
    }
}

func main() {
    jobs := make(chan int, NUM_JOBS)
    var wg sync.WaitGroup

    // Start workers
    for w := 1; w <= NUM_WORKERS; w++ {
        wg.Add(1)
        go worker(w, jobs, &wg)
    }

    // Send jobs to the workers
    for j := 1; j <= NUM_JOBS; j++ {
        jobs <- j
    }
    
    close(jobs) // Close the jobs channel when done sending

    wg.Wait() // Wait for all workers to finish
}
```

### Explanation

- **Worker Function**: The `worker` function processes jobs received from the `jobs` channel.
- **Fixed Number of Workers**: A specified number of workers (goroutines) are created at startup. Each worker listens on the same `jobs` channel.
- **Job Distribution**: Jobs are sent into the `jobs` channel. When a worker is free, it picks up a job from the channel and processes it.

### 3. Using `errgroup` with SetLimit (from `golang.org/x/sync/errgroup`)

The `errgroup` package provides a way to manage a group of goroutines and allows you to set limits on concurrent operations.

#### Example Code

```go
package main

import (
	"fmt"
	"golang.org/x/sync/errgroup"
	"math/rand"
	"time"
)

func main() {
	var g errgroup.Group
	sem := make(chan struct{}, 2) // Limit to 2 concurrent operations

	for i := 1; i <= 10; i++ {
		i := i // capture range variable
		g.Go(func() error {
			sem <- struct{}{} // Acquire semaphore
			defer func() { <-sem }() // Release semaphore

			fmt.Printf("Processing job %d\n", i)
			time.Sleep(time.Duration(rand.Intn(3)) * time.Second) // Simulate work
			fmt.Printf("Completed job %d\n", i)

			return nil
		})
	}

	if err := g.Wait(); err != nil {
		fmt.Println("Error:", err)
	}
}
```

### Explanation

- **Semaphore Channel**: A buffered channel `sem` is used as a semaphore to limit concurrent operations.
- **errgroup.Group**: The `errgroup` package manages multiple goroutines and waits for them to complete.
- **Go Method**: The `g.Go()` method starts each job while managing concurrency through the semaphore.

### Conclusion

Limiting the number of goroutines in Go can be effectively achieved using buffered channels, worker pools, or third-party libraries like `errgroup`. Each method has its advantages and can be chosen based on specific requirements such as simplicity or advanced error handling. These patterns help prevent overwhelming system resources and ensure that your application remains responsive and efficient under load.

Citations:
[1] https://calmops.com/golang/golang-limit-total-number-of-goroutines/
[2] https://www.reddit.com/r/golang/comments/16vpbua/limiting_number_of_goroutines/
[3] https://dev.to/reprintsev/how-to-limit-the-number-of-simultaneously-running-goroutines-and-wait-for-their-completion-12pf
[4] https://dev.to/crusty0gphr/tricky-golang-interview-questions-part-8-max-goroutine-number-1ep2
[5] https://github.com/tidwall/limiter