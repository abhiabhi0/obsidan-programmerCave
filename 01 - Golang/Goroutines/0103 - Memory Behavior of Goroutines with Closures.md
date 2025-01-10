#### Key Insights

1. **Memory Sharing with Closures**:
    
    - Goroutines operate within the same address space as the main program. When a closure is run inside a goroutine, it captures variables from its lexical scope.
    - If a goroutine modifies a variable from its surrounding scope, it directly affects the original variable.
2. **Example of Variable Modification by a Closure**:
    
    ```go
    var wg sync.WaitGroup
    salutation := "hello"
    wg.Add(1)
    go func() {
        defer wg.Done()
        salutation = "welcome"
    }()
    wg.Wait()
    fmt.Println(salutation)  // Output: welcome
    ```
    
    - In this example, the goroutine modifies `salutation`. The modification affects the original variable, demonstrating shared memory space.

#### Loop Variables and Unexpected Output

3. **Closure Capturing Loop Variables**:
    
    ```go
    var wg sync.WaitGroup
    for _, salutation := range []string{"hello", "greetings", "good day"} {
        wg.Add(1)
        go func() {
            defer wg.Done()
            fmt.Println(salutation)
        }()
    }
    wg.Wait()
    ```
    
    - Intuitively, this seems like it would print each salutation in some order. Instead, it often prints the last iteration’s value multiple times (e.g., `good day` three times).
4. **Why This Happens**:
    
    - The loop variable `salutation` is reused in each iteration.
    - Goroutines reference the same memory address for `salutation`. By the time the goroutines run, the loop has completed, and `salutation` holds its final value.
    - Closures in Go are lexically scoped. This means that any variables referenced within the closure from the "outer" scope are not a copy but are in fact a reference.
    
_This is not the case now. It was till Go 1.22_
https://go.dev/doc/faq#closures_and_goroutines
#### How Go Manages Memory for Closures

5. **Heap Allocation for Closure Variables**:
    - When the Go runtime detects that a variable is still referenced by a goroutine, it moves the variable from the stack to the heap. This prevents premature garbage collection, ensuring goroutines can safely access the variable.

#### Correcting the Loop Variable Issue

6. **Solution: Passing Loop Variables as Parameters**:
    
    ```go
    var wg sync.WaitGroup
    for _, salutation := range []string{"hello", "greetings", "good day"} {
        wg.Add(1)
        go func(s string) {
            defer wg.Done()
            fmt.Println(s)
        }(salutation)
    }
    wg.Wait()
    ```
    
    - **Explanation**:
        - By passing `salutation` as an argument to the goroutine’s anonymous function, a copy of the current iteration’s value is created.
        - Each goroutine now works with a unique copy rather than referencing the same memory location.
7. **Correct and Expected Output**:
    
    - Output could be:
        
        ```
        hello
        greetings
        good day
        ```
        
    - Order may vary, but each value prints exactly once.

#### Synchronization Considerations

8. **Shared Address Space**:
    - Since goroutines share memory, care must be taken to synchronize access using either:
        - **Memory Synchronization (sync.Mutex)** for shared variables.
        - **Communication via Channels** to avoid direct sharing of memory.

#### Conclusion

- Goroutines, by capturing variables within closures, share the same memory unless explicitly copied.
- Go’s runtime moves referenced variables to the heap when necessary, allowing safe access but requiring developers to manage synchronization explicitly.

#### Diagram for Variable Capturing

1. Before Correction:
    
    ```plaintext
    +--------------------------------+
    | Loop Iteration:                |
    | "hello" -> goroutine          |
    | "greetings" -> goroutine      |
    | "good day" -> goroutine      |
    | salutation (shared) ---------> "good day"
    +--------------------------------+
    ```
    
2. After Correction:
    
    ```plaintext
    +-------------------------------------+
    | Loop Iteration:                     |
    | "hello" -> goroutine (copy: "hello") |
    | "greetings" -> goroutine (copy: "greetings") |
    | "good day" -> goroutine (copy: "good day") |
    +-------------------------------------+
    ```

Ref: Concurrency in Go (p40-p43)