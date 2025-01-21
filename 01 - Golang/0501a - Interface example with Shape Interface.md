```go
package main

import (
	"fmt"
	"math"
)

// Define the Shape interface
type Shape interface {
	Area() float64
	Perimeter() float64
}

// Circle struct implementing Shape interface
type Circle struct {
	radius float64
}

func (c Circle) Area() float64 {
	return math.Pi * c.radius * c.radius
}

func (c Circle) Perimeter() float64 {
	return 2 * math.Pi * c.radius
}

// Rectangle struct implementing Shape interface
type Rectangle struct {
	length, width float64
}

func (r Rectangle) Area() float64 {
	return r.length * r.width
}

func (r Rectangle) Perimeter() float64 {
	return 2 * (r.length + r.width)
}

// Function to print shape details
func printShapeDetails(s Shape) {
	fmt.Printf("Area: %f, Perimeter: %f\n", s.Area(), s.Perimeter())
}

func main() {
	var s Shape

	s = Circle{radius: 5}
	printShapeDetails(s)

	s = Rectangle{length: 4, width: 3}
	printShapeDetails(s)
}
```