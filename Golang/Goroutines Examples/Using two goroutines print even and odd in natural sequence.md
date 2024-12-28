```go
package main
 
import "fmt"
 
func printOdd(ch chan int, done chan bool) {
	for {
		fmt.Println("odd:  ", <-ch)
		done <- true
	}
}
 
func printEven(ch chan int, done chan bool) {
	for {
		fmt.Println("even: ", <-ch)
		done <- true
	}
}
 
func main() {
	oddCh := make(chan int)
	evenCh := make(chan int)
	done := make(chan bool)
	go printOdd(oddCh, done)
	go printEven(evenCh, done)
	for i := 1; i <= 10; i++ {
		if i%2 == 0 {
			evenCh <- i
		} else {
			oddCh <- i
		}
<-done
	}
}
```

