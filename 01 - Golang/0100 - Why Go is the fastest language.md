### 1. Compiled Language
- Go is a **compiled language**, meaning that it translates source code directly into machine code without relying on an interpreter or virtual machine. This direct compilation results in faster execution times compared to languages that use bytecode or are interpreted, such as Java or Python [1][4][6].

### 2. Efficient Concurrency
- Go has built-in support for **concurrency** through goroutines and channels. Goroutines are lightweight threads that allow developers to run multiple functions simultaneously without significant overhead. They occupy only about 2 KB of memory each, enabling the execution of thousands or even millions of goroutines concurrently [5][7]. This makes Go particularly suitable for high-load applications and I/O-bound tasks.
- [[0100 - Goroutines]]

### 3. Garbage Collection
- Go features an efficient **garbage collector** that minimizes latency and optimizes memory management. This allows Go applications to adapt quickly to large-scale concurrent systems, which is crucial for maintaining performance in high-concurrency environments [3][4].
- [[0301 - Garbage Collector in Go]]
	
### 4. Simplicity and Readability
- The language's design emphasizes simplicity and clarity, allowing developers to write less complex code that is easier to maintain and optimize. This can lead to better performance overall, as simpler code often translates to fewer bugs and more efficient execution [2][6].

### 5. Native Compilation
- Unlike languages like Java, which compile to bytecode for execution on a virtual machine, Go compiles directly to native machine code. This eliminates the overhead associated with virtual machines, resulting in faster application startup times and improved runtime performance [4][6].

### 6. Optimized for Modern Hardware
- Go is designed to take full advantage of modern CPU architectures, including multi-core processors, which enhances its ability to handle concurrent operations effectively [1][7].

### 7. Fast Compile Times
- Go is known for its rapid compile times, which can significantly speed up the development process. This allows developers to iterate quickly and test their applications without long wait times [4][6].
- [[Go’s Static Typing and Single Binary Deployment]]

https://web.stanford.edu/class/ee380/Abstracts/100428-pike-stanford.pdf

Citations:
[1] https://www.orientsoftware.com/blog/golang-performance/
[2] https://www.scalefocus.com/blog/why-you-should-go-with-go-for-your-next-software-project
[3] https://www.mytaskpanel.com/go-programming-language/
[4] https://www.bairesdev.com/blog/why-golang-is-so-fast-performance-analysis/
[5] https://www.devlane.com/blog/should-you-use-golang-advantages-disadvantages-examples
[6] https://bestarion.com/what-is-golang/
[7] https://www.tftus.com/blog/why-opt-for-golang-the-benefits-of-choosing-golang-for-your-project