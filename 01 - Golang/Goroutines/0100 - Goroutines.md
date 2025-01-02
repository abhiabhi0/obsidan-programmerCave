## What are Goroutines?

- **Definition**: A goroutine is a lightweight execution thread managed by the Go runtime. It allows functions to run concurrently with other goroutines, enabling efficient multitasking within a program [1][2][3].

- **Creation**: To create a goroutine, you simply prefix a function call with the `go` keyword. For example:
  ```go
  go myFunction()
  ```
  This initiates `myFunction` to run concurrently with the rest of the program [2][5].

- **Main Goroutine**: Every Go program starts with a single goroutine known as the main goroutine. If this main goroutine exits, all other goroutines are terminated as well [1][6].

## Characteristics of Goroutines

- **Lightweight**: Goroutines are much lighter than traditional threads. They consume less memory and incur lower overhead when being created or destroyed, allowing thousands of them to run simultaneously without significant resource consumption [2][3][4].

- **Communication via Channels**: Goroutines can communicate with each other using channels, which provide a safe way to share data between them without locks. Channels are bidirectional, meaning they can send and receive data [1][4].

- **Concurrency Model**: In Go, concurrency is achieved through goroutines, enabling multiple processes to run at the same time. This is particularly useful for I/O-bound tasks and applications that require high performance [5][6].
[[0101 - Concurrency in Go]]

Citations:
[1] https://www.scaler.com/topics/golang/goroutines/
[2] https://www.educative.io/answers/what-is-a-goroutine
[3] https://dev.to/sanjaysinghrajpoot/what-are-goroutines-in-golang-19mh
[4] https://dev.to/jeffotoni/a-little-about-goroutines-in-go-2f0f
[5] https://www.programiz.com/golang/goroutines
[6] https://www.geeksforgeeks.org/goroutines-concurrency-in-golang/