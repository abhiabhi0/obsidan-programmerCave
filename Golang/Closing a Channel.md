```go
ch <- 1
ch <- 2
ch <- 3
close(ch) // Closes a channel
```

```go
// Iterate the channel until closed
for i := range ch {
  ···
}
```

```go
// Closed if `ok == false`
v, ok := <- ch
```
