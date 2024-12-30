In Go, signals are a way for the operating system to notify a running program about various events, such as interruptions, termination requests, or other types of notifications. The `os/signal` package in the Go standard library provides mechanisms to handle these signals in a Go program.

### Common Use Cases for Signals in Go

1. **Graceful Shutdown**: Handling termination signals (`SIGINT`, `SIGTERM`) to perform cleanup operations before the program exits.
2. **Reloading Configuration**: Responding to signals like `SIGHUP` to reload configuration files without restarting the application.
3. **Debugging**: Handling signals like `SIGQUIT` to perform debugging tasks, such as printing stack traces.

### Basic Usage of Signals in Go

Here is an example of how to handle OS signals in Go:

```go
package main

import (
    "fmt"
    "os"
    "os/signal"
    "syscall"
)

func main() {
    // Create a channel to receive OS signals
    sigs := make(chan os.Signal, 1)
    
    // Notify the channel of specified signals
    signal.Notify(sigs, syscall.SIGINT, syscall.SIGTERM)

    // Create a channel to signal when the program should exit
    done := make(chan bool, 1)

    // Start a goroutine to handle signals
    go func() {
        sig := <-sigs
        fmt.Println("Received signal:", sig)
        done <- true
    }()

    fmt.Println("Waiting for signal")
    <-done
    fmt.Println("Exiting")
}
```

### Explanation of the Code

1. **Create a channel to receive OS signals**:
    ```go
    sigs := make(chan os.Signal, 1)
    ```
    - This channel will be used to receive incoming signals from the OS.

2. **Notify the channel of specified signals**:
    ```go
    signal.Notify(sigs, syscall.SIGINT, syscall.SIGTERM)
    ```
    - The `signal.Notify` function registers the channel to receive notifications for the specified signals. Here, it registers for `SIGINT` (interrupt signal, typically sent by Ctrl+C) and `SIGTERM` (termination signal).

4. **Create a channel to signal when the program should exit**:
    ```go
    done := make(chan bool, 1)
    ```
    - This channel is used to signal the main goroutine to exit after handling the signal.

5. **Start a goroutine to handle signals**:
    ```go
    go func() {
        sig := <-sigs
        fmt.Println("Received signal:", sig)
        done <- true
    }()
    ```
    - A goroutine is started to listen for signals on the `sigs` channel. When a signal is received, it prints the signal and sends a `true` value to the `done` channel.

6. **Main goroutine waits for a signal**:
    ```go
    fmt.Println("Waiting for signal")
    <-done
    fmt.Println("Exiting")
    ```
    - The main goroutine waits for a value on the `done` channel. When a signal is handled, the value is received, and the program prints "Exiting" and then exits.


- A typical pattern involves creating a channel to receive signals, registering the channel with `signal.Notify`, and handling the signals in a goroutine.
