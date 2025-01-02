#### Step 1: Define the Data Structures

We will define a `Node` struct for the doubly linked list and an `LRUCache` struct that will manage the cache.

```go
package main

import (
    "container/list"
    "fmt"
    "sync"
)

// Node represents an entry in the cache
type Node struct {
    key   int
    value string
}

// LRUCache represents the LRU cache structure
type LRUCache struct {
    capacity int
    cache    map[int]*list.Element // Map to hold keys and their corresponding linked list node
    list     *list.List            // Doubly linked list to maintain order of usage
    mu       sync.RWMutex          // Mutex for thread-safe access
}

// NewLRUCache initializes a new LRUCache with the specified capacity
func NewLRUCache(capacity int) *LRUCache {
    return &LRUCache{
        capacity: capacity,
        cache:    make(map[int]*list.Element),
        list:     list.New(),
    }
}
```

#### Step 2: Implement Cache Methods

We will implement methods for getting and putting items in the cache, as well as a method to evict the least recently used item when the cache exceeds its capacity.

#### Get Method

The `Get` method retrieves an item from the cache and updates its position in the list to mark it as recently used.

```go
// Get retrieves an item from the cache
func (lru *LRUCache) Get(key int) (string, bool) {
    lru.mu.RLock()
    defer lru.mu.RUnlock()

    if element, found := lru.cache[key]; found {
        // Move the accessed node to the front of the list
        lru.list.MoveToFront(element)
        return element.Value.(*Node).value, true
    }
    return "", false // Key not found
}
```

#### Put Method

The `Put` method adds or updates an item in the cache. If the cache exceeds its capacity, it evicts the least recently used item.

```go
// Put adds an item to the cache
func (lru *LRUCache) Put(key int, value string) {
    lru.mu.Lock()
    defer lru.mu.Unlock()

    if element, found := lru.cache[key]; found {
        // Update existing node and move it to front
        element.Value.(*Node).value = value
        lru.list.MoveToFront(element)
        return
    }

    // Create new node
    newNode := &Node{key: key, value: value}
    newElement := lru.list.PushFront(newNode)
    lru.cache[key] = newElement

    // Check if we need to evict an item
    if lru.list.Len() > lru.capacity {
        // Remove the least recently used item from both list and map
        backElement := lru.list.Back()
        if backElement != nil {
            lru.list.Remove(backElement)
            delete(lru.cache, backElement.Value.(*Node).key)
        }
    }
}
```

### Step 3: Example Usage

Here’s how you can use the `LRUCache` in your main function:

```go
func main() {
    lru := NewLRUCache(2) // Set capacity to 2

    lru.Put(1, "Product 1")
    fmt.Println(lru.Get(1)) // Outputs: Product 1 true

    lru.Put(2, "Product 2")
    fmt.Println(lru.Get(2)) // Outputs: Product 2 true

    lru.Put(3, "Product 3") // Evicts key 1 (Product 1)

    // Key 1 should be evicted now
    if _, found := lru.Get(1); !found {
        fmt.Println("Key '1' not found (evicted)") // Outputs: Key '1' not found (evicted)
    }

    fmt.Println(lru.Get(2)) // Outputs: Product 2 true
    fmt.Println(lru.Get(3)) // Outputs: Product 3 true

    lru.Put(4, "Product 4") // Evicts key 2 (Product 2)

    if _, found := lru.Get(2); !found {
        fmt.Println("Key '2' not found (evicted)") // Outputs: Key '2' not found (evicted)
    }

    fmt.Println(lru.Get(3)) // Outputs: Product 3 true
    fmt.Println(lru.Get(4)) // Outputs: Product 4 true
}
```

### Explanation of Key Components

- **Doubly Linked List**: Used to maintain the order of items based on their usage. The most recently used items are at the front, while the least recently used items are at the back.
- **Hash Map**: Provides O(1) access time for retrieving items based on their keys.
- **Mutex**: Ensures thread-safe operations on the cache when accessed by multiple goroutines.

### Conclusion

This implementation provides a basic but effective LRU Cache in Go. It efficiently manages memory by evicting the least recently used items when reaching its capacity limit. You can further enhance this implementation by adding features such as expiration times or integrating it with other data structures depending on your application's needs.