```go
func table_n(n int, wg *sync.WaitGroup, ch chan int) {
    defer wg.Done()
    for {
        i, open := <-ch
        if !open {
            fmt.Println("Channel closed!")
            break
        }
        fmt.Println(i * n)
        if i == 10 {
            return
        }
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

    wg.Wait()
}
```