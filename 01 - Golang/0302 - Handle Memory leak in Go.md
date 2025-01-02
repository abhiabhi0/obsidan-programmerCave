[[0300 - Memory Management in Go]]
[[0301 - Garbage Collector in Go]]
#### What Are Memory Leaks?
- **Definition**: Memory leaks occur when a program allocates memory but fails to release it, even when it is no longer needed.
- **Impact**:
    - Leads to increased memory consumption over time.
    - Can cause performance degradation and potential application crashes.

#### How Memory Leaks Happen in Go

1. **Long-Lived References**:
    - Objects with active references cannot be garbage collected, even if no longer needed.
    - Example:
        
        ```go
        var cache = map[string][]byte{}
        
        func cacheData(key string, data []byte) {
            cache[key] = data
        }
        
        func main() {
            for i := 0; i < 1000000; i++ {
                data := make([]byte, 1024) // 1 KB
                cacheData(fmt.Sprintf("%d", i), data)
            }
            select {} // Prevent exit
        }
        ```
    - Issue: Data is added to the cache without a removal strategy.

2. **Goroutines**:    
    - Goroutines that are blocked or never terminate can lead to memory leaks.
    - Example:
        ```go
        func leakyFunc(c chan int) {
            val := <-c
            fmt.Println(val)
        }
        
        func main() {
            for {
                ch := make(chan int)
                go leakyFunc(ch)
            }
        }
        ```
        
3. **Caching Without Bounds**:
    - Indefinitely growing caches consume memory unless an eviction strategy is implemented.
    
1. **Cgo or Unsafe Code**:
    - Improper usage can bypass the garbage collector.
    - Example:
        ```go
        // #include <stdlib.h>
        import "C"
        
        func main() {
            mem := C.malloc(1000) // Allocate memory using C's malloc
            // Forgot to free, hence memory is leaked
            // Correct way: C.free(mem)
        }
        ```
        

#### Identifying Memory Leaks in Go

- **Profiling Tools**:
    - Use the `pprof` package for runtime behavior analysis.
    - Capture heap dumps by integrating `net/http/pprof`.
    - Analyze with `go tool pprof` (text or visual formats).
- **Indicators**:
    - Consistent memory growth over time.
    - Growth during idle periods can indicate leaks.

#### Preventing Memory Leaks in Go

1. **Mind Your References**:
    - Dereference objects when they are no longer needed.
    - Avoid unintended references in global variables, maps, or channels.
    - Example with cache eviction:
        ```go
        var cache = map[string]struct {
            data      []byte
            timestamp time.Time
        }{}
        
        const ttl = 10 * time.Second
        
        func cacheData(key string, data []byte) {
            cache[key] = struct {
                data      []byte
                timestamp time.Time
            }{
                data:      data,
                timestamp: time.Now(),
            }
        }
        
        func evictOldEntries() {
            for key, item := range cache {
                if time.Since(item.timestamp) > ttl {
                    delete(cache, key)
                }
            }
        }
        
        func main() {
            ticker := time.NewTicker(time.Second)
            for range ticker.C {
                evictOldEntries()
            }
        }
        ```
        
2. **Use Bounded Caches**:
    - Implement eviction strategies like LRU (Least Recently Used) or TTL (Time To Live).
    
1. **Monitor Goroutines**:
    - Ensure goroutines terminate properly.
    - Use `runtime.NumGoroutine()` to monitor active goroutines.
    - Example:
        ```go
        func worker(ctx context.Context, ch chan int) {
            for {
                select {
                case val := <-ch:
                    fmt.Println(val)
                case <-ctx.Done():
                    return
                }
            }
        }
        
        func main() {
            ch := make(chan int)
            ctx, cancel := context.WithCancel(context.Background())
            go worker(ctx, ch)
        
            // Signal the worker goroutine to stop
            cancel()
        }
        ```
        
4. **Avoid Finalizers**:
    - Finalizers delay memory reclamation and can introduce unexpected behavior.

#### Summary

- Memory leaks can occur in Go despite its garbage collection mechanism.
- Causes include long-lived references, improperly managed goroutines, unbounded caches, and misuse of Cgo.
- Profiling and monitoring tools are essential for identifying leaks.
- Preventive measures like proper reference management, bounded caching, goroutine monitoring, and avoiding finalizers help mitigate leaks effectively.