```go
func fanIn(ch1, ch2 <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for {
            select {
            case v := <-ch1:
                out <- v
            case v := <-ch2:
                out <- v
            }
        }
    }()
    return out
}

func main() {
    ch1 := make(chan int)
    ch2 := make(chan int)

    go func() {
        for i := 0; i < 5; i++ {
            ch1 <- i
        }
        close(ch1)
    }()

    go func() {
        for i := 5; i < 10; i++ {
            ch2 <- i
        }
        close(ch2)
    }()

    for v := range fanIn(ch1, ch2) {
        fmt.Println(v)
    }
}

```