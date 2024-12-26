```go
package main

import (
	"fmt"
	"sync"
)

func printOdd(o chan bool, e chan bool, wg *sync.WaitGroup) {
	defer wg.Done()
	for i := 1; i <= 10; i += 2 {
		<-o
		fmt.Println(i)
		e <- true
	}
}

func printEven(o chan bool, e chan bool, wg *sync.WaitGroup) {
	defer wg.Done()
	for i := 2; i <= 10; i += 2 {
		<-e
		fmt.Println(i)
		if i < 10 {
			o <- true
		}
	}
}

func main() {
	var wg sync.WaitGroup
	o := make(chan bool)
	e := make(chan bool)

	wg.Add(2)
	go printOdd(o, e, &wg)
	go printEven(o, e, &wg)

	o <- true // Start the sequence by signaling the first goroutine

	wg.Wait()
}
```

