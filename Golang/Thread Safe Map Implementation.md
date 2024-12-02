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
