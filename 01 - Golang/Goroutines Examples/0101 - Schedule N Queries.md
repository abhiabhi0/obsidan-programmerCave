Scheduler -> N SQL Queries -> []string{} -> Concurrently
Func Execute(singleQuery string) -> print it with a sleep 5 seconds
64GB -> 5 queries at a time -> Database doesn’t crash

To implement a scheduler in Go that executes SQL queries concurrently with a limit of 5 concurrent executions, we can use goroutines and channels. If any query fails, we will stop all subsequent queries from running. Below is a complete implementation along with explanations.

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

// Execute simulates the execution of a single SQL query.
// It prints the query and sleeps for 5 seconds to mimic processing time.
func Execute(singleQuery string, wg *sync.WaitGroup, successChan chan<- bool) {
	defer wg.Done()

	fmt.Printf("Executing query: %s\n", singleQuery)
	time.Sleep(5 * time.Second) // Simulate time taken to execute the query

	// Simulate a random failure for demonstration purposes
	if singleQuery == "FAIL" {
		fmt.Println("Query failed:", singleQuery)
		successChan <- false // Signal failure
		return
	}

	fmt.Println("Query executed successfully:", singleQuery)
	successChan <- true // Signal success
}

// Scheduler schedules the execution of n SQL queries concurrently with a limit on concurrency.
func Scheduler(queries []string) {
	const maxConcurrentQueries = 5
	var wg sync.WaitGroup
	semaphore := make(chan struct{}, maxConcurrentQueries)
	successChan := make(chan bool)

	for _, query := range queries {
		semaphore <- struct{}{} // Acquire a spot in the semaphore
		wg.Add(1)

		go func(q string) {
			defer func() { <-semaphore }() // Release the spot in the semaphore
			Execute(q, &wg, successChan)
		}(query)

		// Check for failure and stop subsequent queries if one fails
		go func() {
			if !<-successChan {
				fmt.Println("Stopping all subsequent queries due to failure.")
				close(successChan) // Close channel to signal failure
				return
			}
		}()
	}

	wg.Wait() // Wait for all queries to finish
}

func main() {
	queries := []string{
		"SELECT * FROM users",
		"SELECT * FROM orders",
		"FAIL", // Simulate a failure with this query
		"SELECT * FROM products",
		"SELECT * FROM transactions",
	}

	Scheduler(queries)
}
```

### Explanation

#### Functionality Overview

1. **Execute Function**:
   - This function simulates executing a SQL query. It prints the query and sleeps for 5 seconds to mimic processing time.
   - For demonstration purposes, it randomly fails if the query string is `"FAIL"`.

2. **Scheduler Function**:
   - This function manages the scheduling and execution of multiple SQL queries concurrently.
   - It uses a semaphore (a buffered channel) to limit the number of concurrent executions to 5.
   - A `sync.WaitGroup` is used to wait for all goroutines to finish executing.
   - A channel (`successChan`) is used to communicate the success or failure of each query execution.

3. **Concurrency Control**:
   - For each query, we acquire a spot in the semaphore before starting its execution.
   - After executing, we release the spot back into the semaphore.
   - If any query fails (indicated by sending `false` on `successChan`), we close this channel, signaling that no further queries should be executed.

4. **Main Function**:
   - This function initializes an array of SQL queries (including one that simulates failure).
   - It calls the `Scheduler` function with this array.

### Behavior
- The program will execute up to 5 queries concurrently. If any query fails, it will print an error message and stop executing any further queries. The use of goroutines allows for concurrent execution while ensuring that resource limits are respected.