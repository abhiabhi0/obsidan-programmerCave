### **Defer** (Resource cleanup):

1. **Definition**: "The `defer` keyword schedules a function to be executed after the surrounding function completes. It’s commonly used for cleanup tasks like closing files, unlocking resources, or releasing locks."
    
2. **Key Points**:
    
    - "Defer statements are executed in **LIFO (Last In, First Out)** order."
    - "The arguments of a deferred function are evaluated when the `defer` statement is executed, not when the function actually runs."
3. **Example**: "For instance, we use `defer` to ensure a file gets closed, even if an error occurs during file processing."
```go
file, err := os.Open("example.txt")
if err != nil {
    log.Fatal(err)
}
defer file.Close()  // Ensures the file gets closed when the function exits
```

```go
func p() {
	fmt.Println("p")
	defer fmt.Println("defer in p")
	fmt.Println("q")
}

func main() {
	fmt.Println("Start")
	defer fmt.Println("Deferred execution")
	p()
	fmt.Println("End")
}
```

```
Start
p
q
defer in p
End
Deferred execution
```

### **Panic** (Error signaling):

1. **Definition**: "A `panic` in Go is used to indicate an unexpected or fatal error that the program cannot recover from. Once a panic occurs, the normal execution of the program stops, and the deferred functions are executed before the program exits."
    
2. **Key Points**:
    - "Panic is often used for errors that are impossible to recover from, such as invalid function arguments or internal logic errors."
    - "If not recovered, the panic causes the program to terminate with a stack trace."
    
1. **Example**: "For example, we may `panic` when an application encounters an invalid state that we cannot handle."
```go
panic("invalid state")
```

```go
func main() {
    fmt.Println("Before panic")
    panic("Something went wrong!")
    fmt.Println("This will not execute")
}
```

```
Before panic
panic: Something went wrong!
```

### **Recover** (Graceful error handling):

1. **Definition**: "`recover` is used inside a deferred function to regain control of a panicking goroutine. It allows you to handle the panic gracefully instead of letting the program crash."
    
2. **Key Points**:
    - "It only works if called inside a `defer` function."
    - "Recover can stop the panic and allow the program to continue executing."
    
1. **Example**: "For example, we use `recover` to prevent a program from crashing and handle errors more gracefully."
```go
package main

import "fmt"

func main() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from panic:", r)
        }
    }()
    fmt.Println("Before panic")
    panic("An error occurred")
    fmt.Println("This will not execute")
}
```

```
Before panic
Recovered from panic: An error occurred
```

