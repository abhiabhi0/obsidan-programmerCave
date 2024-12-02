```go
package main

import (
	"fmt"
	"sync"
)

type SafeMap struct {
	mu   sync.Mutex
	data map[string]int
}

func NewSafeMap() *SafeMap {
	return &SafeMap{
		data: make(map[string]int),
	}
}

func (m *SafeMap) Set(key string, value int) {
	m.mu.Lock()
	defer m.mu.Unlock()
	m.data[key] = value
}

func (m *SafeMap) Get(key string) (int, bool) {
	m.mu.Lock()
	defer m.mu.Unlock()
	value, ok := m.data[key]
	return value, ok
}

func main() {
	m := NewSafeMap()
	m.Set("a", 42)
	value, ok := m.Get("a")
	if ok {
		fmt.Println("Value:", value)
	} else {
		fmt.Println("Key not found")
	}
}
```

### Using sync.Map

`sync.Map` is a concurrent map provided by Go's `sync` package. It is optimized for high-concurrency use cases and doesn't require manual locking. Key features:

#### Characteristics:

1. **Thread-safe**: Multiple goroutines can safely access it without extra locking.
2. **No Generics**: Stores `interface{}` types, so you must cast values.
3. **Optimized for Writes**: Better performance in scenarios with frequent updates.
4. **Iterate with Care**: Iteration doesn't guarantee order or consistency across modifications.

#### Common Methods:

- **`Store(key, value)`**: Adds or updates a key-value pair.
- **`Load(key)`**: Retrieves the value for a key.
- **`Delete(key)`**: Removes a key-value pair.
- **`LoadOrStore(key, value)`**: Loads an existing value or stores and returns a new one.
- **`Range(func(key, value any) bool)`**: Iterates over all entries.

#### Example:

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var sm sync.Map

	// Store values
	sm.Store("a", 1)
	sm.Store("b", 2)

	// Load values
	if value, ok := sm.Load("a"); ok {
		fmt.Println("Key 'a':", value)
	}

	// Iterate
	sm.Range(func(key, value any) bool {
		fmt.Println("Key:", key, "Value:", value)
		return true // Continue iteration
	})

	// Delete a key
	sm.Delete("b")
}
```

#### Use Cases:

- High-concurrency applications with frequent reads/writes.
- Caches or shared state in concurrent systems.