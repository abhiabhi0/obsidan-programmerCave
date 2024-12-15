### Summary
#### Key Features:
1. **Timeout Management**: Ensures all API requests are canceled after 5 seconds.
2. **Cancellation Propagation**: Cancels remaining requests if the context is canceled.
#### How It Works:
1. **Context Setup**:
    - `context.WithTimeout` creates a context with a timeout of `5s`.
    - `defer cancel()` ensures cleanup of resources when the timeout occurs.
2. **API Fetching**:
    - Each URL is processed concurrently in `fetchAPI` using goroutines.
    - The `http.NewRequestWithContext` ensures the request respects the context.
3. **Result Collection**:
    - Results from all APIs are gathered and printed.
#### Advantages:
- Graceful request handling with timeout and cancellation.
- Efficient resource utilization in concurrent environments.

Context provides a mechanism to control the lifecycle, cancellation, and propagation of requests across multiple goroutines.

---
# Example: Managing Concurrent API Requests

Consider a scenario where you need to fetch data from multiple APIs concurrently. By using context, you can ensure that all the API requests are canceled if any of them exceeds a specified timeout.

```go
func main() {  
	ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)  
	defer cancel()  
	  
	urls := []string{  
	"https://api.example.com/users",  
	"https://api.example.com/products",  
	"https://api.example.com/orders",  
	}  
	  
	results := make(chan string)  
	  
	for _, url := range urls {  
	go fetchAPI(ctx, url, results)  
	}  
	  
	for range urls {  
	fmt.Println(<-results)  
	}  
}  
  
func fetchAPI(ctx context.Context, url string, results chan<- string) {  
	req, err := http.NewRequestWithContext(ctx, "GET", url, nil)  
	if err != nil {  
	results <- fmt.Sprintf("Error creating request for %s: %s", url, err.Error())  
	return  
	}  
	  
	client := http.DefaultClient  
	resp, err := client.Do(req)  
	if err != nil {  
	results <- fmt.Sprintf("Error making request to %s: %s", url, err.Error())  
	return  
	}  
	defer resp.Body.Close()  
	  
	results <- fmt.Sprintf("Response from %s: %d", url, resp.StatusCode)  
}
```