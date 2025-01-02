Design patterns are essential tools in software development that provide standard solutions to common problems. In Go, design patterns can be implemented effectively due to the language's features like interfaces, goroutines, and channels. Below is an overview of some common design patterns in Go, along with brief explanations and examples.

### 1. Creational Patterns

These patterns deal with object creation mechanisms, trying to create objects in a manner suitable to the situation.

#### **Singleton Pattern**
The Singleton pattern ensures that a class has only one instance and provides a global point of access to it.

**Example:**
```go
package main

import (
    "fmt"
    "sync"
)

type singleton struct{}

var instance *singleton
var once sync.Once

func GetInstance() *singleton {
    once.Do(func() {
        instance = &singleton{}
    })
    return instance
}

func main() {
    for i := 0; i < 10; i++ {
        go func() {
            fmt.Println(GetInstance())
        }()
    }
}
```

#### **Factory Method**
This pattern provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created.

**Example:**
```go
package main

import "fmt"

type Shape interface {
    Draw()
}

type Circle struct{}

func (c *Circle) Draw() {
    fmt.Println("Drawing a Circle")
}

type Square struct{}

func (s *Square) Draw() {
    fmt.Println("Drawing a Square")
}

func ShapeFactory(shapeType string) Shape {
    if shapeType == "circle" {
        return &Circle{}
    }
    return &Square{}
}

func main() {
    shape1 := ShapeFactory("circle")
    shape1.Draw()

    shape2 := ShapeFactory("square")
    shape2.Draw()
}
```

#### **Builder Pattern**
The Builder pattern separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

**Example:**
```go
package main

import "fmt"

type Car struct {
    Make  string
    Model string
}

type CarBuilder struct {
    car Car
}

func (b *CarBuilder) SetMake(make string) *CarBuilder {
    b.car.Make = make
    return b
}

func (b *CarBuilder) SetModel(model string) *CarBuilder {
    b.car.Model = model
    return b
}

func (b *CarBuilder) Build() Car {
    return b.car
}

func main() {
    builder := &CarBuilder{}
    car := builder.SetMake("Toyota").SetModel("Corolla").Build()
    
    fmt.Printf("Car Make: %s, Model: %s\n", car.Make, car.Model)
}
```

### 2. Structural Patterns

These patterns deal with object composition and typically help ensure that if one part of a system changes, the entire system doesn't need to change.

#### **Adapter Pattern**
The Adapter pattern allows incompatible interfaces to work together by wrapping an existing class with a new interface.

**Example:**
```go
package main

import "fmt"

type OldSystem struct{}

func (o *OldSystem) OldMethod() {
    fmt.Println("Old method called")
}

type NewSystem interface {
    NewMethod()
}

type Adapter struct {
    oldSystem *OldSystem
}

func (a *Adapter) NewMethod() {
    a.oldSystem.OldMethod()
}

func main() {
    old := &OldSystem{}
    adapter := &Adapter{oldSystem: old}
    
    adapter.NewMethod() // Outputs: Old method called
}
```

### 3. Behavioral Patterns

These patterns deal with object interaction and responsibility.

#### **Observer Pattern**
The Observer pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

**Example:**
```go
package main

import "fmt"

type Observer interface {
	Update(string)
}

type Subject struct {
	observers []Observer
	state     string
}

func (s *Subject) Attach(o Observer) {
	s.observers = append(s.observers, o)
}

func (s *Subject) Notify() {
	for _, observer := range s.observers {
		observer.Update(s.state)
	}
}

func (s *Subject) SetState(state string) {
	s.state = state
	s.Notify()
}

type ConcreteObserver struct{}

func (c *ConcreteObserver) Update(state string) {
	fmt.Printf("Observer updated with state: %s\n", state)
}

func main() {
	subject := &Subject{}
	observer := &ConcreteObserver{}

	subject.Attach(observer)
	subject.SetState("New State") // Outputs: Observer updated with state: New State
}
```

### Conclusion

These examples illustrate how various design patterns can be implemented in Go. Each pattern serves a specific purpose and can help improve code organization, reusability, and maintainability. Understanding these patterns can significantly enhance your ability to design robust applications in Go.

Citations:
[1] https://refactoring.guru/design-patterns/go
[2] https://dwarvesf.hashnode.dev/common-design-patterns-in-golang-part-1
[3] https://dev.to/krpmuruga/golang-design-patterns-2lo
[4] https://www.linkedin.com/pulse/go-8-essential-design-patterns-every-programmer-must-know-nitin-singh-rw2bc