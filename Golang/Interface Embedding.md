In Go, interface embedding refers to the practice of composing a new interface by including one or more existing interfaces within it. This allows the new interface to inherit the methods of the embedded interfaces, thereby providing a form of interface inheritance and promoting code reuse.

Here's a step-by-step explanation:

1. **Basic Interface Definition**:
   - An interface in Go is a collection of method signatures.
   - Any type that implements these methods satisfies the interface.

2. **Embedding Interfaces**:
   - When you embed an interface within another interface, the methods of the embedded interface become part of the new interface.
   - This means that any type implementing the new interface must implement the methods of all embedded interfaces.

3. **Advantages of Interface Embedding**:
   - Promotes code reuse by allowing you to build more complex interfaces from simpler ones.
   - Helps in creating more modular and maintainable code.
   - Provides a way to extend interfaces without modifying existing code.

### Example

Let's consider a simple example to illustrate interface embedding.

1. **Define Basic Interfaces**:

```go
package main

import "fmt"

// Basic interfaces
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}
```

2. **Embed Interfaces to Create a New Interface**:

```go
// New interface that embeds Reader and Writer
type ReadWriter interface {
    Reader
    Writer
}
```

3. **Implementing the New Interface**:

```go
// A type that implements both Read and Write methods
type MyReadWriter struct{}

func (rw MyReadWriter) Read(p []byte) (n int, err error) {
    fmt.Println("Reading data")
    return len(p), nil
}

func (rw MyReadWriter) Write(p []byte) (n int, err error) {
    fmt.Println("Writing data")
    return len(p), nil
}

func main() {
    var rw ReadWriter = MyReadWriter{}

    // Using the ReadWriter interface
    data := make([]byte, 10)
    rw.Read(data)
    rw.Write(data)
}
```

### Explanation:
1. **Basic Interfaces**: `Reader` and `Writer` are two simple interfaces with one method each.
2. **New Interface**: `ReadWriter` is a new interface that embeds both `Reader` and `Writer`. This means any type that wants to satisfy `ReadWriter` must implement both `Read` and `Write` methods.
3. **Type Implementation**: `MyReadWriter` is a concrete type that implements the methods required by both `Reader` and `Writer`. Thus, it also satisfies the `ReadWriter` interface.
4. **Usage**: In the `main` function, an instance of `MyReadWriter` is used as a `ReadWriter`. The embedded methods are called via the `ReadWriter` interface.

This example demonstrates how interface embedding in Go allows for the composition of more complex interfaces from simpler ones, facilitating code reuse and maintainability.