```go
func doWork(ctx context.Context) {
	select {
	case <-time.After(3 * time.Second):
		fmt.Println("Work completed")
	case <-ctx.Done():
		fmt.Println("Work cancelled:", ctx.Err())
	}
}

func main() {
	ctx, cancel := context.WithCancel(context.Background())

	go doWork(ctx)

	time.Sleep(1 * time.Second) // Simulate some work in main
	cancel()                     // Cancel the work

	time.Sleep(1 * time.Second) // Wait to see the cancellation message
}

```