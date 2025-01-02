[[0400 - Concurrency Patterns]]

```go
func worker(id int, jobs <-chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    for job := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, job)
    }
}

func main() {
    const numWorkers = 3
    jobs := make(chan int, 10)
    var wg sync.WaitGroup

    // Start workers
    for w := 1; w <= numWorkers; w++ {
        wg.Add(1)
        go worker(w, jobs, &wg)
    }

    // Send jobs
    for j := 1; j <= 10; j++ {
        jobs <- j
    }
    close(jobs) // Close the channel to signal no more jobs

    wg.Wait() // Wait for all workers to finish
}
```