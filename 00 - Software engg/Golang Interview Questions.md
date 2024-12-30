### why is it used?
- statically typed and compiled programming language
- combines the ease of use of a high-level language with the performance of a low-level language. 
- syntax is easy to understand and its standard library provides a wide range of functionalities.
- built-in support for concurrency, making it ideal for developing applications that require dealing with multiple tasks simultaneously.

### How do you implement concurrency in Go?
- concurrency is implemented using goroutines and channels. 
- Goroutines are lightweight threads that can be created with the go keyword. They allow concurrent execution of functions.
- Channels, on the other hand, are used for communication and synchronization between goroutines. They can be created using make and can be used to send and receive values.

### How do you handle errors in Go?
- errors are handled using the error type. 
- When a function encounters an error, it can return an error value indicating the problem. The calling code can then check if the error is nil. If not, it handles the error appropriately.
- Go provides a built-in panic function to handle exceptional situations. When a panic occurs, it stops the execution of the program and starts unwinding the stack, executing deferred functions along the way.

### Interfaces
 - Define Interface
```go
// Define the interface
type Speaker interface {
    Speak() string
}
```
- create a struct type (or use any existing type) that has all the methods required by the interface.
```go
// Define the struct
type Person struct {
    Name string
}

// Implement the Speak method for Person
func (p Person) Speak() string {
    return "Hello, my name is " + p.Name
}
```
- Now that the `Person` struct has a `Speak` method, it implicitly implements the `Speaker` interface. You can create a variable of type `Speaker` and assign an instance of `Person` to it.
```go
func main() {
    // Create an instance of Person
    person := Person{Name: "Alice"}

    // Declare a variable of type Speaker and assign the person to it
    var speaker Speaker = person

    // Use the Speak method via the interface
    fmt.Println(speaker.Speak())
}
```
- In Go, there's no need to explicitly declare that `Person` implements the `Speaker` interface. The Go compiler automatically recognizes that `Person` satisfies the `Speaker` interface because it has a `Speak` method with the correct signature. This is what is meant by "implicit implementation" of interfaces in Go.

### What is the role of the "init" function in Go?
 - special function in Go that is used to initialize global variables or perform any other setup tasks needed by a package before it is used. 
 - called automatically when the package is first initialized. Its execution order within a package is not guaranteed.
 - Multiple init functions can be defined within a single package and even within a single file. 
 - This allows for modular and flexible initialization of package-level resources.
```go
// Global variable
var globalVar string

// init function to initialize global variables
func init() {
    globalVar = "Initialized in init"
    fmt.Println("init function called")
}

// Another init function
func init() {
    fmt.Println("Another init function")
}

func main() {
    fmt.Println("main function called")
    fmt.Println("Global variable:", globalVar)
}
```
- The `init` functions are called before the `main` function.
- If multiple `init` functions are defined within the same package, they are executed in the order they appear.
- The `init` function does not take any parameters and does not return any values.
- Each file within a package can have its own `init` function, and all `init` functions across the package will be executed.

### What are Golang packages?
- Go Packages (abbreviated pkg) are simply directories in the Go workspace that contain.
- Go source files or other Go packages. 
- Every piece of code created in the source files, from variables to functions, is then placed in a linked package. 
- Every source file should be associated with a package.

### Const in Go?
- const Pi = 3.14
- The value must be a compile-time constant such as a string, number, or boolean.
- After defining a constant, you can use it in your code throughout the program. 
- Note that constants cannot be reassigned or modified during the execution of the program.

### Channels
- used to communicate between goroutines. 
- Channels can be either buffered or unbuffered, each serving different purposes and behaviors.

#### Unbuffered Channels
- do not have any capacity to hold values
- require both a sender and a receiver to be ready at the same time for the communication to take place. 
- This makes unbuffered channels suitable for synchronizing goroutines.
```go
func main() {
    // Create an unbuffered channel
    ch := make(chan int)

    // Start a goroutine
    go func() {
        ch <- 42 // Send value to the channel
        fmt.Println("Sent 42 to the channel")
    }()

    // Receive value from the channel
    value := <-ch
    fmt.Println("Received", value)
}
```

#### Buffered Channels
 - have a capacity defined when they are created. 
 - can hold a limited number of values before blocking the sender. 
 - This allows a sender to send multiple values without waiting for a receiver, up to the channel's capacity.
```go
func main() {
    // Create a buffered channel with capacity of 2
    ch := make(chan int, 2)

    // Send values to the channel
    ch <- 42
    fmt.Println("Sent 42 to the channel")

    ch <- 43
    fmt.Println("Sent 43 to the channel")

    // Receive values from the channel
    value1 := <-ch
    fmt.Println("Received", value1)

    value2 := <-ch
    fmt.Println("Received", value2)
}
```

1. **Blocking Behavior**:
   - **Unbuffered Channel**: The sender blocks until the receiver is ready to receive the value, and the receiver blocks until the sender is ready to send the value.
   - **Buffered Channel**: The sender blocks only if the buffer is full, and the receiver blocks only if the buffer is empty.

2. **Synchronization**:
   - **Unbuffered Channel**: Synchronizes the sender and receiver, ensuring both are ready at the same time.
   - **Buffered Channel**: Allows for some decoupling between sender and receiver, providing a buffer to hold intermediate values.

3. **Usage Scenarios**:
   - **Unbuffered Channel**: Useful for scenarios where you need strict synchronization between goroutines.
   - **Buffered Channel**: Useful for scenarios where you want to allow the sender to proceed without waiting for the receiver, up to a certain point.
### Goroutine
 - A Goroutine is a function or procedure that runs concurrently with other Goroutines on a dedicated Goroutine thread. 
 - Goroutine threads are lighter than ordinary threads, and most Golang programs use thousands of goroutines at the same time.  
 - A Goroutine can be stopped by passing it a signal channel. 
 - Because Goroutines can only respond to signals if they are taught to check, you must put checks at logical places, such as at the top of your for a loop.
```go
// Function that runs as a goroutine
func worker(stopChan chan bool) {
    for {
        select {
        case <-stopChan:
            fmt.Println("Stopping goroutine")
            return
        default:
            fmt.Println("Working...")
            time.Sleep(500 * time.Millisecond)
        }
    }
}

func main() {
    // Create a stop channel
    stopChan := make(chan bool)

    // Start the worker goroutine
    go worker(stopChan)

    // Let the worker run for some time
    time.Sleep(2 * time.Second)

    // Send a signal to stop the goroutine
    stopChan <- true

    // Give some time to ensure the goroutine has stopped
    time.Sleep(1 * time.Second)
    fmt.Println("Main function exiting")
}
```

- Using `select`, a goroutine can react to multiple channel operations, effectively handling multiple concurrent tasks.
```go
select {
case val := <-ch1:
    fmt.Println("Received from ch1:", val)
case ch2 <- 42:
    fmt.Println("Sent to ch2")
default:
    fmt.Println("No communication")
}
```

### Advantages of Golang
#### Simplified Dependency Management
**Go Modules**:
- Go modules are the standard way to manage dependencies in Go. A module is a collection of related Go packages that are versioned together.
- Go modules define dependencies using a `go.mod` file, which lists the module’s dependencies and their versions.
**Automatic Dependency Resolution**:
- Go automatically fetches and resolves dependencies specified in the `go.mod` file.
- When you build or run a Go program, the Go toolchain ensures that all required dependencies are downloaded and available.
#### Fully Garbage-Collected
**Automatic Memory Management**:
- Go includes a garbage collector that automatically manages memory allocation and deallocation.
- garbage collector runs in the background, identifying which objects are no longer in use and reclaiming their memory.
- Go uses a generational garbage collection approach, which is optimized for performance by categorizing objects based on their lifespan.

### Pointers
```go
var a int = 42
var p *int = &a
*p = 100
```

### Arrays
```go
var arr [3]int
```

- After declaring the array, you can then initialize it by assigning values to each index. 
- You can also access and modify elements of the array using their index number.
- Arrays in Go have fixed sizes, meaning you cannot add or remove elements once they are declared.

### How will you perform inheritance with Golang?
- Golang does not support classes, hence there is no inheritance.  
- However, you may use composition to imitate inheritance behavior by leveraging an existing struct object to establish the initial behavior of a new object. 
- Once the new object is created, the functionality of the original struct can be enhanced.
- Go uses composition, where one struct can include other structs as fields.
```go
// Define the Person struct
type Person struct {
    Name string
    Age  int
}

// Define the Employee struct that embeds Person
type Employee struct {
    Person    // Embed Person struct
    Position  string
    Salary    float64
}
```

### Slice
```go
mySlice := make([]string, 5)
```
- The length of the slice is not fixed and can be changed dynamically as elements are added or removed.
- You can access and modify the elements in the slice using their index.

### defer statement
- used to postpone the execution of a function until the surrounding function completes.
- It is often used when you want to make sure some cleanup tasks are performed before exiting a function, regardless of errors or other conditions.
```go
func main() {
	defer cleanup()
}

func cleanup() {
	fmt.Println("performing cleanup")
}
```

### Mutex
- A mutex, short for "mutual exclusion," is a synchronization primitive used to protect shared resources in concurrent programming. 
- In Go, the `sync` package provides a `Mutex` type that helps prevent race conditions by ensuring that only one goroutine can access a critical section of code at a time.
```go
// Counter is a struct that holds an integer value and a mutex
type Counter struct {
    value int
    mux   sync.Mutex
}

// Increment safely increments the counter
func (c *Counter) Increment() {
    c.mux.Lock()         // Lock the mutex
    c.value++            // Critical section
    c.mux.Unlock()       // Unlock the mutex
}

// Value returns the current value of the counter
func (c *Counter) Value() int {
    c.mux.Lock()         // Lock the mutex
    defer c.mux.Unlock() // Ensure the mutex is unlocked after returning
    return c.value
}

func main() {
    var wg sync.WaitGroup
    counter := Counter{}

    // Create 1000 goroutines to increment the counter
    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            counter.Increment()
        }()
    }

    // Wait for all goroutines to finish
    wg.Wait()

    // Print the final value of the counter
    fmt.Println("Final Counter Value:", counter.Value())
}
```

- **Prevent Race Conditions**: Ensures that shared resources are accessed by only one goroutine at a time, preventing concurrent access issues.
- **Simplicity**: Provides a straightforward way to synchronize access to critical sections of code.
- **Safety**: Reduces the risk of data corruption in concurrent programs.

### context
- By using the "context" package, you can pass request-scoped values throughout your Go application easily.
- Context provides a mechanism to control the lifecycle, cancellation, and propagation of requests across multiple goroutines.

### Concurrency vs Parallelism
- Concurrency refers to multiple tasks being executed simultaneously,  
- parallelism involves splitting a task into smaller sub-tasks and running them simultaneously.