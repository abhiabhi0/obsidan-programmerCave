
Golang, while not a purely object-oriented language, supports several OOP principles through its unique features. Below are detailed notes on the core OOP concepts in Golang, along with practical examples and diagrams to aid your understanding.

### Core Principles of OOP

1. **Encapsulation**
   - **Definition**: Encapsulation restricts access to certain details of an object and only exposes necessary parts.
   - **Implementation in Golang**: Go uses exported and unexported identifiers. If a variable name starts with an uppercase letter, it is exported (public); if it starts with a lowercase letter, it is unexported (private).
   - **Example**:
     ```go
     type Car struct {
         id   int    // unexported
         Name string // exported
     }

     func (c *Car) GetID() int {
         return c.id
     }

     func (c *Car) GetName() string {
         return c.Name
     }
     ```
   - **Diagram**:
     ```
     +-----------------+
     |      Car        |
     +-----------------+
     | - id: int      |  <- private
     | + Name: string |  <- public
     +-----------------+
     ```

2. **Abstraction**
   - **Definition**: Abstraction involves hiding complex implementation details and showing only the essential features of an object.
   - **Implementation in Golang**: Achieved through interfaces, which define methods that must be implemented by any type that satisfies the interface.
   - **Example**:
     ```go
     type Vehicle interface {
         Drive() string
         Stop() string
     }

     type BMW struct {
         Model string
     }

     func (b BMW) Drive() string {
         return "Driving " + b.Model
     }

     func (b BMW) Stop() string {
         return "Stopping " + b.Model
     }
     
     func DriveVehicle(v Vehicle) {
         fmt.Println(v.Drive())
         fmt.Println(v.Stop())
     }
     ```

3. **Inheritance**
   - **Definition**: Inheritance allows one class to inherit the properties and methods of another class, promoting code reuse.
   - **Implementation in Golang**: Go does not support traditional inheritance but allows struct embedding.
   - **Example**:
     ```go
     type Vehicle struct {
         Seats int
         Color string
     }

     type Car struct {
         Vehicle // embedding Vehicle struct
         Model   string
     }

     func main() {
         car := Car{Vehicle: Vehicle{Seats: 4, Color: "blue"}, Model: "BMW"}
         fmt.Println(car.Seats) // Output: 4
         fmt.Println(car.Color) // Output: blue
     }
     ```

4. **Polymorphism**
   - **Definition**: Polymorphism allows objects to be treated as instances of their parent class, enabling a single interface to represent different underlying forms (data types).
   - **Implementation in Golang**: Achieved through interfaces, where different types can implement the same interface.
   - **Example**:
     ```go
     type Animal interface {
         Sound() string
     }

     type Dog struct{}
     
     func (d Dog) Sound() string {
         return "Woof!"
     }

     type Cat struct{}
     
     func (c Cat) Sound() string {
         return "Meow!"
     }

     func MakeSound(a Animal) {
         fmt.Println(a.Sound())
     }
     
     func main() {
         var animal Animal = Dog{}
         MakeSound(animal) // Output: Woof!
         
         animal = Cat{}
         MakeSound(animal) // Output: Meow!
     }
     ```

### Additional Concepts for Interview Preparation

- **Interfaces vs Structs**:
  - Interfaces define behavior (methods), while structs define data (fields).
  - Interfaces allow for polymorphic behavior where different types can be used interchangeably.

- **Concurrency in Golang**:
  - Understand goroutines and channels for concurrent programming.
  - Be prepared to discuss how Go handles concurrency compared to other languages.

- **Common Interview Questions**:
  - Explain the difference between maps and slices.
  - Discuss how Go manages memory allocation and garbage collection.
  - Describe how you would handle error management in Go.

### Tips for Success

- Practice coding examples related to each OOP principle.
- Familiarize yourself with common interview questions and their answers.
- Review Go's concurrency model, as it often comes up in interviews.

By mastering these concepts and practicing coding examples, you'll be well-prepared for your interview on OOP in Golang. Good luck!

Citations:
[1] https://www.linkedin.com/pulse/oop-principles-golang-shariful-islam
[2] https://www.reddit.com/r/golang/comments/pzrp64/golang_interview_preparation/
[3] https://faun.pub/oops-concept-in-golang-4c403fcac911?gi=eb7ac8612c2f
[4] https://www.guvi.com/blog/golang-interview-questions-and-answers/
[5] https://dev.to/adriandy89/understanding-golang-object-oriented-programming-oop-with-examples-15l6
[6] https://www.simplilearn.com/golang-interview-questions-article
[7] https://www.geeksforgeeks.org/object-oriented-programming-in-golang/
[8] https://www.turing.com/interview-questions/golang
[9] https://dev.to/parthlaw/object-oriented-go-unraveling-the-power-of-oop-in-golang-49h6
[10] https://www.reddit.com/r/golang/comments/16zvxv4/golangs_approach_to_objectoriented_programming/