## What are Interfaces?

Interfaces in Go are types that define a set of method signatures. They specify what methods a type must implement but do not provide the implementation itself. This allows for **polymorphism** and **loose coupling** in your code.

### Key Characteristics of Interfaces:
- **Implicit Implementation**: A type implements an interface simply by providing the methods defined in that interface. There is no explicit declaration required (e.g., no `implements` keyword).
- **Flexibility**: Interfaces allow different types to be treated as the same type if they implement the same methods.
- **Decoupling**: They promote separation between the definition of functionality and its implementation.

### Example of an Interface:

```go
type Shape interface {
    Area() float64
    Perimeter() float64
}
```

In this example, any type that implements `Area` and `Perimeter` methods satisfies the `Shape` interface.

## Implementing Interfaces

To implement an interface, a type must define all the methods declared by that interface. Here’s how you can do it:

### Example Implementation:

```go
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}
```

In this case, `Rectangle` implements the `Shape` interface by providing definitions for both `Area` and `Perimeter`.

## Using Interfaces

Interfaces can be used to write functions that accept different types as long as they implement the required methods. This is particularly useful for writing flexible and reusable code.

### Example Function Using Interface:

```go
func PrintShapeInfo(s Shape) {
    fmt.Printf("Area: %f\n", s.Area())
    fmt.Printf("Perimeter: %f\n", s.Perimeter())
}
```

Here, `PrintShapeInfo` can accept any type that implements the `Shape` interface.

## Type Assertion and Type Switch

When working with interfaces, you may need to determine the underlying type of an interface value. This can be done using **type assertions** or **type switches**.

### Type Assertion Example:

```go
var s Shape = Rectangle{Width: 5, Height: 3}
if rect, ok := s.(Rectangle); ok {
    fmt.Println("Rectangle area:", rect.Area())
}
```

### Type Switch Example:

```go
switch v := s.(type) {
case Rectangle:
    fmt.Println("Rectangle with area:", v.Area())
case Circle:
    fmt.Println("Circle with area:", v.Area())
default:
    fmt.Println("Unknown shape")
}
```

## Common Interview Questions

1. **What are interfaces used for?**
   - They are used to define behaviors that multiple types can implement, enabling polymorphism.

2. **How do you implement an interface in Go?**
   - By defining all methods declared in the interface within a type.

3. **Can you explain implicit vs explicit implementation?**
   - In Go, interfaces are implemented implicitly; no keyword is needed to declare that a type implements an interface.

4. **What is a nil interface?**
   - A nil interface value holds no concrete type and is often used to indicate absence of value or behavior.

5. **How do interfaces promote loose coupling?**
   - By allowing different implementations to be swapped without changing code that depends on the interface.

## Diagrams

### Interface Structure

```plaintext
+-------------------+
|      Shape        |
|-------------------|
| + Area() float64  |
| + Perimeter() float64 |
+-------------------+
         ^
         |
+-------------------+
|   Rectangle       |
|-------------------|
| + Area()          |
| + Perimeter()     |
+-------------------+
```

This diagram illustrates how the `Rectangle` type implements the `Shape` interface.

## Conclusion

Understanding interfaces is crucial for writing idiomatic Go code. Focus on their characteristics, implementation patterns, and how they enable polymorphism in your applications. Good luck with your interview!

Citations:
[1] https://www.reddit.com/r/golang/comments/pzrp64/golang_interview_preparation/
[2] https://www.simplilearn.com/golang-interview-questions-article
[3] https://www.educative.io/blog/50-golang-interview-questions
[4] https://www.guvi.in/blog/golang-interview-questions-and-answers/
[5] https://www.weekday.works/post/golang-interview-questions
[6] https://in.linkedin.com/in/yashcr7
[7] https://www.softkraft.co/top-15-golang-interview-questions/
[8] https://www.geeksforgeeks.org/interfaces-in-golang/
[9] https://www.turing.com/interview-questions/golang