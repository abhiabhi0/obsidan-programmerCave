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

