### Implementation of Cache with TTL

```go
// CacheItem represents an item in the cache with its expiration time.
type CacheItem struct {
    Value      string
    Expiration time.Time
}

// Cache structure with TTL support
type Cache struct {
    data map[int]CacheItem
    mu   sync.RWMutex
    ttl  time.Duration // Default TTL for cache items
}

// NewCache initializes a new cache with a specified default TTL.
func NewCache(ttl time.Duration) *Cache {
    return &Cache{
        data: make(map[int]CacheItem),
        ttl:  ttl,
    }
}

// Get retrieves data from the cache and checks for expiration.
func (c *Cache) Get(key int) (string, bool) {
    c.mu.RLock()
    defer c.mu.RUnlock()

    item, found := c.data[key]
    if !found || time.Now().After(item.Expiration) {
        return "", false // Item not found or expired
    }
    return item.Value, true // Return value if found and not expired
}

// Set stores data in the cache with a specified TTL.
func (c *Cache) Set(key int, value string) {
    c.mu.Lock()
    defer c.mu.Unlock()

    expiration := time.Now().Add(c.ttl)
    c.data[key] = CacheItem{Value: value, Expiration: expiration}
}

// Cleanup removes expired items from the cache.
func (c *Cache) Cleanup() {
    c.mu.Lock()
    defer c.mu.Unlock()

    for key, item := range c.data {
        if time.Now().After(item.Expiration) {
            delete(c.data, key)
        }
    }
}

// Fetch simulates fetching data from a database.
func Fetch(key int) string {
    time.Sleep(1 * time.Second) // Simulate delay
    return fmt.Sprintf("Product %d", key)
}

func main() {
    cache := NewCache(5 * time.Second) // Set default TTL to 5 seconds

    for i := 1; i <= 2; i++ {
        // Check cache first
        if value, found := cache.Get(i); found {
            fmt.Printf("Cache hit: %s\n", value)
        } else {
            fmt.Printf("Cache miss for key %d\n", i)
            value := Fetch(i)
            cache.Set(i, value) // Store fetched value in cache
            fmt.Printf("Fetched from DB: %s\n", value)
        }
        
        // Wait before checking again
        time.Sleep(2 * time.Second)

        // Check again within TTL period
        if value, found := cache.Get(i); found {
            fmt.Printf("Cache hit after waiting: %s\n", value)
        } else {
            fmt.Printf("Cache miss after waiting for key %d\n", i)
        }

        // Wait longer than TTL to demonstrate expiration
        time.Sleep(6 * time.Second)

        // Cleanup expired items (optional)
        cache.Cleanup()

        // Check after TTL expiration
        if value, found := cache.Get(i); found {
            fmt.Printf("Cache hit after TTL expired: %s\n", value)
        } else {
            fmt.Printf("Cache miss after TTL expired for key %d\n", i)
        }
    }
}
```

### Explanation of the Code

1. **CacheItem Struct**:
   - Represents an item stored in the cache along with its expiration time.

2. **Cache Structure**:
   - Contains a map to store `CacheItem` entries and a mutex (`sync.RWMutex`) to ensure thread-safe access.
   - The `ttl` field defines the default Time-To-Live for each item.

3. **NewCache Function**:
   - Initializes a new instance of `Cache` with a specified default TTL.

4. **Get Method**:
   - Retrieves an item from the cache and checks if it has expired. Returns the item's value and a boolean indicating whether it was found.

5. **Set Method**:
   - Stores an item in the cache along with its expiration time based on the default TTL.

6. **Cleanup Method**:
   - Removes any expired items from the cache. This can be called periodically to ensure that stale entries are cleared.

7. **Fetch Function**:
   - Simulates fetching data from an external source (e.g., a database).

8. **Main Function**:
   - Demonstrates how to use the caching mechanism by checking for cached values, fetching them if not present, and observing expiration behavior.

### Conclusion

This implementation provides a basic framework for a caching system in Go that supports Time-To-Live (TTL). It allows you to efficiently manage cached items while ensuring that stale data is automatically removed after a specified duration. This pattern is useful in scenarios where you want to minimize database access while keeping your application responsive.

Citations:
[1] https://github.com/devnw/ttl
[2] https://pkg.go.dev/github.com/ReneKroon/ttlcache
[3] https://github.com/jellydator/ttlcache
[4] https://dev.to/leoantony72/go-high-performance-cache-with-ttl-and-disk-persistence-4a4m