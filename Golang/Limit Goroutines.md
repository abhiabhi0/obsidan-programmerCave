```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func worker(id int, wg *sync.WaitGroup, sem chan bool) {
	defer wg.Done()
	
	// Acquire a token from the semaphore
	sem <- true
	
	fmt.Printf("Worker %d starting\n", id)
	time.Sleep(2 * time.Second) // Simulate work
	fmt.Printf("Worker %d done\n", id)
	
	// Release the token back to the semaphore
	<-sem
}

func main() {
	const maxGoroutines = 3
	var wg sync.WaitGroup

	// Create a buffered channel of boolean values to act as a semaphore
	sem := make(chan bool, maxGoroutines)

	for i := 1; i <= 10; i++ {
		wg.Add(1)
		go worker(i, &wg, sem)
	}

	wg.Wait()
	fmt.Println("All workers done")
}

```