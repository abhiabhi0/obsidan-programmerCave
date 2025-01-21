```go
package main

import "fmt"

// Define the Worker interface
type Worker interface {
	Work()
}

// Person struct implementing Worker interface
type Person struct {
	name string
}

func (p Person) Work() {
	fmt.Println(p.name, "is working")
}

// Function to describe a worker
func describe(w Worker) {
	fmt.Printf("Interface type %T value %v\n", w, w)
}

func main() {
	p := Person{name: "Alice"}
	var w Worker = p // Implicitly implements the Worker interface

	describe(w)
	w.Work()
}
```