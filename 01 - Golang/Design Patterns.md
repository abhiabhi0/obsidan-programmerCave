
Go (Golang) supports many design patterns that are commonly used in software engineering. Here, I'll illustrate a few key design patterns using Go: Singleton, Factory, and Observer.

### Singleton Pattern

The Singleton pattern ensures that a class has only one instance and provides a global point of access to it.

```go
package main

import (
	"fmt"
	"sync"
)

type Singleton struct {
}

var instance *Singleton
var once sync.Once

func GetInstance() *Singleton {
	once.Do(func() {
		instance = &Singleton{}
	})
	return instance
}

func main() {
	s1 := GetInstance()
	s2 := GetInstance()

	if s1 == s2 {
		fmt.Println("Singleton works, both variables contain the same instance.")
	} else {
		fmt.Println("Singleton failed, variables contain different instances.")
	}
}
```
- `once` is a variable of type `sync.Once` from the `sync` package. It is used to ensure that the initialization of the `Singleton` instance happens only once.
- Inside this function, we use `once.Do` to ensure that the block of code inside it runs only once.
    - `once.Do` takes a function as an argument. In this case, the function creates a new `Singleton` and assigns its address to the `instance` variable.
- After `once.Do` is called, the `instance` is returned. This means any subsequent calls to `GetInstance` will return the same `instance`.
### Factory Pattern

The Factory pattern provides a way to create objects without specifying the exact class of object that will be created.

```go
package main

import "fmt"

// Shape is the interface that all concrete shapes implement
type Shape interface {
	Draw()
}

// Circle is a concrete shape that implements Shape
type Circle struct{}

func (c Circle) Draw() {
	fmt.Println("Drawing Circle")
}

// Square is a concrete shape that implements Shape
type Square struct{}

func (s Square) Draw() {
	fmt.Println("Drawing Square")
}

// ShapeFactory is the factory that creates shapes
type ShapeFactory struct{}

func (sf ShapeFactory) GetShape(shapeType string) Shape {
	if shapeType == "Circle" {
		return Circle{}
	} else if shapeType == "Square" {
		return Square{}
	}
	return nil
}

func main() {
	factory := ShapeFactory{}

	shape1 := factory.GetShape("Circle")
	shape1.Draw()

	shape2 := factory.GetShape("Square")
	shape2.Draw()
}
```

### Observer Pattern

The Observer pattern is used to notify a list of objects about a change in the state of another object.

```go
package main

import "fmt"

// Subject is the subject that is being observed
type Subject struct {
	observers []Observer
	state     int
}

// Observer is the interface for all observers
type Observer interface {
	Update(state int)
}

// Attach adds an observer to the subject
func (s *Subject) Attach(observer Observer) {
	s.observers = append(s.observers, observer)
}

// SetState changes the state of the subject and notifies observers
func (s *Subject) SetState(state int) {
	s.state = state
	s.notifyAllObservers()
}

// notifyAllObservers notifies all attached observers of a state change
func (s *Subject) notifyAllObservers() {
	for _, observer := range s.observers {
		observer.Update(s.state)
	}
}

// ConcreteObserverA is a concrete observer that implements the Observer interface
type ConcreteObserverA struct {
	name string
}

func (coa ConcreteObserverA) Update(state int) {
	fmt.Printf("Observer %s: State changed to %d\n", coa.name, state)
}

func main() {
	subject := &Subject{}

	observer1 := ConcreteObserverA{name: "A"}
	observer2 := ConcreteObserverA{name: "B"}

	subject.Attach(observer1)
	subject.Attach(observer2)

	subject.SetState(1)
	subject.SetState(2)
}
```

### Summary

- **Singleton Pattern**: Ensures only one instance of a class exists and provides a global access point.
- **Factory Pattern**: Creates objects without specifying the exact class of object to be created.
- **Observer Pattern**: Notifies a list of observers about changes in the state of another object.

These examples demonstrate how you can implement these design patterns in Go. Each pattern addresses different aspects of software design, promoting code reuse and flexibility.