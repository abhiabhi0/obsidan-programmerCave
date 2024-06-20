```go
func compute() {
	for i := 0; i < 10000; i++ {
		_ = "hello"
	}
}

func main() {
	var wg sync.WaitGroup
	numGoroutines := 8
	totalCalls := 100
	callsPerRoutine := totalCalls / numGoroutines
	extraCalls := totalCalls % numGoroutines

	for i := 0; i < numGoroutines; i++ {
		wg.Add(1)
		go func(n int) {
			defer wg.Done()
			for j := 0; j < n; j++ {
				compute()
			}
		}(callsPerRoutine + func() int { if i < extraCalls { return 1 } else { return 0 } }())
	}

	wg.Wait()
}
```

- **Distributing Calls**: In the loop, we assign each goroutine `callsPerRoutine` calls, plus an extra call if the index `i` is less than `extraCalls`.
- **Goroutine Execution**: Each goroutine calls the `compute` function the assigned number of times.
- **Waiting for Completion**: The `wg.Wait()` call in the main function blocks until all the goroutines have finished executing.

