```go
type Ball struct{ hits int }

func main() {
    table := make(chan *Ball)
    go player("ping", table)
    go player("pong", table)
    table <- new(Ball) // game on; toss the ball
    time.Sleep(1 * time.Second)
    <-table // game over; grab the ball
}

func player(name string, table chan *Ball) {
    for {
        ball := <-table
        ball.hits++
        fmt.Println(name, ball.hits)
        time.Sleep(100 * time.Millisecond)
        table <- ball
    }
}
```

Ref: [[Closing a Channel]]

This Go program simulates a simple ping-pong game using goroutines and channels. Here's a detailed explanation of how it works:

---

### **Key Concepts**

1. **Goroutines**:
    
    - Lightweight threads managed by the Go runtime.
    - The program uses goroutines for the two players (`ping` and `pong`) to simulate concurrent activities.
2. **Channels**:
    
    - Channels are used for communication between goroutines.
    - Here, a `chan *Ball` is used to pass a pointer to a `Ball` struct between the players.
3. **Synchronization**:
    
    - The program ensures synchronized access to the shared `Ball` using the `table` channel.

---

### **Code Walkthrough**

#### **Struct Definition**

```go
type Ball struct{ hits int }
```

- **`Ball`**: A struct with a single field `hits` that tracks how many times the ball has been hit.

---

#### **Main Function**

```go
func main() {
    table := make(chan *Ball)
```

- **`table`**: A channel of pointers to `Ball`. It acts as the "table" where the ball is exchanged between players.

```go
    go player("ping", table)
    go player("pong", table)
```

- Two goroutines are created, each running the `player` function:
    - One named `"ping"`.
    - The other named `"pong"`.

```go
    table <- new(Ball) // game on; toss the ball
```

- A new `Ball` is created and sent into the `table` channel to start the game.

```go
    time.Sleep(1 * time.Second)
```

- The program sleeps for 1 second to allow the game to proceed.

```go
    <-table // game over; grab the ball
```

- The ball is taken out of the channel to end the game. The program ends after this.

---

#### **Player Function**

```go
func player(name string, table chan *Ball) {
    for {
        ball := <-table
```

- The player function continuously loops (`for {}`).
- Each player waits to receive the ball from the `table` channel.

```go
        ball.hits++
        fmt.Println(name, ball.hits)
```

- When the player receives the ball, they increment the `hits` count.
- The player's name and the updated hit count are printed.

```go
        time.Sleep(100 * time.Millisecond)
```

- A delay of 100 milliseconds is added to simulate time taken to hit the ball.

```go
        table <- ball
```

- The player sends the ball back into the channel (`table`) for the other player to pick up.

---

### **Game Flow**

1. **Start the Game**:
    
    - `main()` creates the `table` channel and starts two players as goroutines.
    - A new `Ball` is created and sent to the `table`.
2. **Players Exchange the Ball**:
    
    - Each player:
        - Receives the ball from the channel.
        - Increments the hit count.
        - Prints their name and the number of hits.
        - Sends the ball back to the channel.
3. **End the Game**:
    
    - After 1 second, `main()` retrieves the ball from the channel, effectively ending the game.

---

### **Sample Output**

The output might look like this:

```
ping 1
pong 2
ping 3
pong 4
ping 5
pong 6
```

- The players alternate, hitting the ball and incrementing its `hits` field.

---

### **Key Points**

1. **Concurrency**:
    
    - The game demonstrates concurrent execution using goroutines.
    - The `table` channel ensures synchronization between the players.
2. **Shared State**:
    
    - The `Ball` is a shared object passed between players via the channel.
3. **Controlled Execution**:
    
    - The `Sleep` calls add delays to simulate real-time gameplay.
4. **Graceful Termination**:
    
    - The game stops after `main()` retrieves the ball from the `table`.

---

### **Possible Interview Questions**

1. **Why use a pointer for `Ball` in the channel?**
    
    - Passing a pointer avoids copying the struct and ensures changes made by one player are visible to the other.
2. **What happens if you don’t retrieve the ball in `main()`?**
    
    - The goroutines would continue indefinitely, causing the program to run indefinitely.
3. **Can this code cause a deadlock?**
    
    - No, because the channel is always properly used to send and receive the ball, ensuring the goroutines stay synchronized.