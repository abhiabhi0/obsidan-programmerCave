```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

func main() {
	// Generate 100 random numbers
	rand.Seed(time.Now().UnixNano())
	numbers := make([]int, 100)
	for i := range numbers {
		numbers[i] = rand.Intn(1000) // Random numbers between 0-999
	}

	// Channels to collect counts
	evenCount := make(chan int)
	oddCount := make(chan int)

	// Use WaitGroup to wait for goroutines to finish
	var wg sync.WaitGroup

	// Function to count evens and odds in a slice
	countEvensAndOdds := func(nums []int, evenCh, oddCh chan int, wg *sync.WaitGroup) {
		defer wg.Done()
		even, odd := 0, 0
		for _, num := range nums {
			if num%2 == 0 {
				even++
			} else {
				odd++
			}
		}
		evenCh <- even
		oddCh <- odd
	}

	// Split the list into chunks for concurrent processing
	chunkSize := 25 // Process 25 numbers per goroutine
	for i := 0; i < len(numbers); i += chunkSize {
		end := i + chunkSize
		if end > len(numbers) {
			end = len(numbers)
		}
		wg.Add(1)
		go countEvensAndOdds(numbers[i:end], evenCount, oddCount, &wg)
	}

	// Goroutine to close channels once counting is done
	go func() {
		wg.Wait()
		close(evenCount)
		close(oddCount)
	}()

	// Collect results from channels
	totalEvens, totalOdds := 0, 0
	for evens := range evenCount {
		totalEvens += evens
	}
	for odds := range oddCount {
		totalOdds += odds
	}

	fmt.Printf("Total Even Numbers: %d\n", totalEvens)
	fmt.Printf("Total Odd Numbers: %d\n", totalOdds)
}

```