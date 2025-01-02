```go
func table_n(n int, wg *sync.WaitGroup, ch chan int) {
	defer wg.Done()
	for {
		i, open := <-ch
		if !open {
			break
		}
		fmt.Println(i * n)
	}
}

func table_channel() {
	ch := make(chan int)
	var wg sync.WaitGroup

	wg.Add(1)
	go table_n(2, &wg, ch)

	for i := 1; i <= 10; i++ {
		ch <- i
	}
	close(ch)

	wg.Wait()
}
```
## Diagram 1: Function Flow

### Execution Flow Diagram

```plaintext
+--------------------+
| table_channel()    |
+--------------------+
         |
         | (Initialize Channel & WaitGroup)
         |
         v
+--------------------+
| wg.Add(1)         |
+--------------------+
         |
         | (Start Goroutine)
         |
         v
+--------------------+
| go table_n(2, wg, ch) |
+--------------------+
         |
         | (Main Loop: Send Values to Channel)
         |
         v
+--------------------+
| for i := 1 to 10   |
|     ch <- i       |
+--------------------+
         |
         | (Close Channel)
         |
         v
+--------------------+
| close(ch)         |
+--------------------+
```

## Diagram 2: Goroutine Interaction

### Goroutine and Channel Interaction

```plaintext
Main Goroutine                        Goroutine (table_n)
+-------------------+                 +-------------------+
| table_channel()   |                 | table_n(2, wg, ch)|
+-------------------+                 +-------------------+
|                   |                 |                   |
|                   |                 |                   |
|      ch <- 1     | ----------------> | read i=1        |
|                   |                 | print 1 * 2 = 2  |
|                   |                 |                   |
|      ch <- 2     | ----------------> | read i=2        |
|                   |                 | print 2 * 2 = 4  |
|                   |                 |                   |
|      ch <- ...    | ----------------> | read i=...      |
|                   |                 | print ...        |
|      ch <- 10    | ----------------> | read i=10       |
|                   |                 | print 10 * 2 = 20|
|                   |                 +-------------------+
|                   |                 
| close(ch)        <-----------------|
| wait for wg      <-----------------|
+-------------------+
```

## Diagram 3: Channel Closure and WaitGroup Completion

### Channel Closure and WaitGroup Completion

```plaintext
Main Goroutine                        Goroutine (table_n)
+-------------------+                 +-------------------+
| table_channel()   |                 | table_n(2, wg, ch)|
+-------------------+                 +-------------------+
|                   |                 |                   |
|      ch <- 1     | ----------------> | read i=1        |
|      ch <- ...    | ----------------> | read i=...      |
|      ch <- 10    | ----------------> | read i=10       |
|                   |                 +-------------------+
| close(ch)        <-----------------|
| wg.Done()        <-----------------|
+-------------------+
```
