```go
func printOdd(ch chan int, done chan bool) {
	for {
		fmt.Println("odd:  ", <-ch)
		done <- true
	}
}
 
func printEven(ch chan int, done chan bool) {
	for {
		fmt.Println("even: ", <-ch)
		done <- true
	}
}
 
func main() {
	oddCh := make(chan int)
	evenCh := make(chan int)
	done := make(chan bool)
	go printOdd(oddCh, done)
	go printEven(evenCh, done)
	for i := 1; i <= 10; i++ {
		if i%2 == 0 {
			evenCh <- i
		} else {
			oddCh <- i
		}
	<-done
	}
}
```

1. **Waiting for Completion**:
    - After sending a number to one of the channels, the main function waits for a signal from the `done` channel.     
```go
<-done
```
        
- This line blocks the execution of the main function until it receives a value from the `done` channel.
### Diagram 1: Structure Overview

```plaintext
+--------------------+
|      Main          |
|                    |
|  +--------------+  |
|  |   oddCh     |  |
|  +--------------+  |
|  +--------------+  |
|  |   evenCh    |  |
|  +--------------+  |
|  +--------------+  |
|  |    done     |  |
|  +--------------+  |
+--------------------+
         |
         | (Start Goroutines)
         v
+--------------------+
|   printOdd        | <--- Receives from oddCh
+--------------------+
         ^
         |
         | (Signals completion)
         |
         v
+--------------------+
|   printEven       | <--- Receives from evenCh
+--------------------+
```

### Diagram 2: Execution Flow

This diagram illustrates how the main function interacts with the `printOdd` and `printEven` goroutines through channels.

```plaintext
Main Function Execution
--------------------------------------------
1. Start Goroutines:
   - go printOdd(oddCh, done)
   - go printEven(evenCh, done)

2. Loop from i = 1 to 10:
   - For i = 1 (odd):
     - Send to oddCh
     - Wait for signal from done
     - <--- "odd: 1" printed by printOdd
     - done <- true

   - For i = 2 (even):
     - Send to evenCh
     - Wait for signal from done
     - <--- "even: 2" printed by printEven
     - done <- true

   - Continue for i = 3 to i = 10...
--------------------------------------------
```

### Diagram 3: Goroutine Interaction

This diagram shows how the goroutines handle incoming numbers and signal completion back to the main function.

```plaintext
Goroutine Interaction
-------------------------------------------------
Main Function                Goroutines
-------------------------------------------------
                              | 
                              v 
          Send odd number to oddCh (e.g., "1")
                              |
                              v 
          Receive from oddCh --> Print "odd: 1"
                              |
                              v 
          Send signal to done channel (done <- true)
                              |
-------------------------------------------------
                              | 
                              v 
          Send even number to evenCh (e.g., "2")
                              |
                              v 
          Receive from evenCh --> Print "even: 2"
                              |
                              v 
          Send signal to done channel (done <- true)
-------------------------------------------------
```

### Summary of Diagrams

1. **Structure Overview**: This diagram shows the main components of your program, including channels and goroutines.
2. **Execution Flow**: This diagram outlines how the main function loops through numbers and interacts with the goroutines.
3. **Goroutine Interaction**: This diagram illustrates how each goroutine processes numbers and sends signals back to the main function.

These diagrams collectively provide a comprehensive view of how your program operates, highlighting the interactions between the main function, goroutines, and channels. By visualizing these relationships, you can better understand how concurrency is managed in your Go code.