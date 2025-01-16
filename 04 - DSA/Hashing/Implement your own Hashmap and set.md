To implement a basic **hashmap** (key-value store) or **hashset** (unique collection of elements), we can leverage **hashing** and **chaining**. Here's a breakdown of how to design a custom implementation in Python:

---

### Key Concepts:

1. **Hash Function:**
    
    - A function that computes an index from the key to determine where to store/retrieve it.
    - Example: `hash(key) % capacity`, where `capacity` is the number of buckets.
2. **Collision Handling:**
    
    - Since multiple keys can hash to the same index, we need a strategy to handle collisions.
    - **Chaining**: Store a list of key-value pairs in each bucket.
3. **Resizing:**
    
    - To maintain performance, the hashmap resizes itself when the load factor (ratio of elements to capacity) exceeds a threshold.
4. **Set Implementation:**
    
    - A hashset is a simplified version of a hashmap where we store only keys, not key-value pairs.

---

### Implementation of HashMap:

```python
class MyHashMap:
    def __init__(self, initial_capacity=16, load_factor=0.75):
        self.capacity = initial_capacity
        self.size = 0
        self.load_factor = load_factor
        self.buckets = [[] for _ in range(self.capacity)]  # Array of lists

    def _hash(self, key):
        """Compute hash index."""
        return hash(key) % self.capacity

    def _resize(self):
        """Resize the hashmap when load factor exceeds the threshold."""
        old_buckets = self.buckets
        self.capacity *= 2
        self.buckets = [[] for _ in range(self.capacity)]
        self.size = 0  # Reset size since `put` will increment it

        for bucket in old_buckets:
            for key, value in bucket:
                self.put(key, value)

    def put(self, key, value):
        """Add or update a key-value pair."""
        if self.size / self.capacity > self.load_factor:
            self._resize()

        index = self._hash(key)
        bucket = self.buckets[index]

        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, value)  # Update existing key
                return

        bucket.append((key, value))  # Add new key-value pair
        self.size += 1

    def get(self, key):
        """Retrieve the value for a given key."""
        index = self._hash(key)
        bucket = self.buckets[index]

        for k, v in bucket:
            if k == key:
                return v

        return None  # Key not found

    def remove(self, key):
        """Remove a key-value pair."""
        index = self._hash(key)
        bucket = self.buckets[index]

        for i, (k, v) in enumerate(bucket):
            if k == key:
                del bucket[i]
                self.size -= 1
                return

    def __str__(self):
        """For debugging: display all key-value pairs."""
        return str({k: v for bucket in self.buckets for k, v in bucket})


# Example Usage
hash_map = MyHashMap()
hash_map.put("apple", 1)
hash_map.put("banana", 2)
hash_map.put("orange", 3)
print(hash_map.get("banana"))  # Output: 2
hash_map.remove("banana")
print(hash_map.get("banana"))  # Output: None
print(hash_map)  # Debugging view
```

---

### Implementation of HashSet:

A **HashSet** is similar but only stores keys.

```python
class MyHashSet:
    def __init__(self, initial_capacity=16, load_factor=0.75):
        self.capacity = initial_capacity
        self.size = 0
        self.load_factor = load_factor
        self.buckets = [[] for _ in range(self.capacity)]

    def _hash(self, key):
        """Compute hash index."""
        return hash(key) % self.capacity

    def _resize(self):
        """Resize the hashset when load factor exceeds the threshold."""
        old_buckets = self.buckets
        self.capacity *= 2
        self.buckets = [[] for _ in range(self.capacity)]
        self.size = 0  # Reset size since `add` will increment it

        for bucket in old_buckets:
            for key in bucket:
                self.add(key)

    def add(self, key):
        """Add a key to the set."""
        if self.size / self.capacity > self.load_factor:
            self._resize()

        index = self._hash(key)
        bucket = self.buckets[index]

        if key not in bucket:
            bucket.append(key)
            self.size += 1

    def contains(self, key):
        """Check if the set contains a key."""
        index = self._hash(key)
        bucket = self.buckets[index]
        return key in bucket

    def remove(self, key):
        """Remove a key from the set."""
        index = self._hash(key)
        bucket = self.buckets[index]

        if key in bucket:
            bucket.remove(key)
            self.size -= 1

    def __str__(self):
        """For debugging: display all keys."""
        return str({key for bucket in self.buckets for key in bucket})


# Example Usage
hash_set = MyHashSet()
hash_set.add(1)
hash_set.add(2)
hash_set.add(3)
print(hash_set.contains(2))  # Output: True
hash_set.remove(2)
print(hash_set.contains(2))  # Output: False
print(hash_set)  # Debugging view
```

---

### Explanation of Code:

1. **Buckets:**
    
    - The hashmap and hashset use an array of lists (buckets) to store values. Each bucket corresponds to an index determined by the hash function.
2. **Hash Function:**
    
    - The `_hash` method calculates the index where the key/value should reside. This ensures uniform distribution across buckets.
3. **Chaining:**
    
    - Each bucket is a list. Collisions are handled by appending multiple elements to the same bucket.
4. **Resizing:**
    
    - If the load factor exceeds a threshold (default 0.75), the data structure doubles its capacity and redistributes all elements to new buckets.
5. **Operations:**
    
    - `put/add`: Insert a key-value pair (or key for hashset).
    - `get/contains`: Retrieve the value for a given key or check if a key exists.
    - `remove`: Remove a key-value pair or a key.

---

### Complexity:

1. **Time Complexity:**
    
    - Average case:
        - Lookup/Insert/Delete: O(1)O(1) (constant time due to the hash function and small bucket size).
    - Worst case:
        - Lookup/Insert/Delete: O(N)O(N) (if all keys hash to the same bucket, degrading to a linked list).
2. **Space Complexity:**
    
    - O(N)O(N), where NN is the number of keys/values.

This implementation provides a foundational understanding of how hashing-based data structures like `dict` and `set` work internally.

---

Here's how you can implement a **HashMap** and **HashSet** in **Go** using similar principles:

---

### Implementation of HashMap in Go

```go
package main

import (
	"fmt"
)

// Entry represents a key-value pair
type Entry struct {
	key   string
	value int
}

// HashMap is a custom hashmap implementation
type HashMap struct {
	buckets    [][]Entry
	capacity   int
	size       int
	loadFactor float64
}

// NewHashMap creates a new HashMap
func NewHashMap(initialCapacity int, loadFactor float64) *HashMap {
	return &HashMap{
		buckets:    make([][]Entry, initialCapacity),
		capacity:   initialCapacity,
		loadFactor: loadFactor,
	}
}

// hash computes the bucket index for a given key
func (h *HashMap) hash(key string) int {
	return int(hashString(key)) % h.capacity
}

// hashString is a simple hash function for strings
func hashString(key string) uint32 {
	hash := uint32(0)
	for _, c := range key {
		hash = 31*hash + uint32(c)
	}
	return hash
}

// Resize doubles the capacity and rehashes all entries
func (h *HashMap) resize() {
	oldBuckets := h.buckets
	h.capacity *= 2
	h.buckets = make([][]Entry, h.capacity)
	h.size = 0

	for _, bucket := range oldBuckets {
		for _, entry := range bucket {
			h.Put(entry.key, entry.value)
		}
	}
}

// Put adds or updates a key-value pair
func (h *HashMap) Put(key string, value int) {
	if float64(h.size)/float64(h.capacity) > h.loadFactor {
		h.resize()
	}

	index := h.hash(key)
	bucket := h.buckets[index]

	// Check if the key exists
	for i, entry := range bucket {
		if entry.key == key {
			h.buckets[index][i].value = value
			return
		}
	}

	// Add a new key-value pair
	h.buckets[index] = append(bucket, Entry{key: key, value: value})
	h.size++
}

// Get retrieves the value for a given key
func (h *HashMap) Get(key string) (int, bool) {
	index := h.hash(key)
	bucket := h.buckets[index]

	for _, entry := range bucket {
		if entry.key == key {
			return entry.value, true
		}
	}

	return 0, false
}

// Remove deletes a key-value pair
func (h *HashMap) Remove(key string) {
	index := h.hash(key)
	bucket := h.buckets[index]

	for i, entry := range bucket {
		if entry.key == key {
			h.buckets[index] = append(bucket[:i], bucket[i+1:]...)
			h.size--
			return
		}
	}
}

// String returns all key-value pairs as a string
func (h *HashMap) String() string {
	result := "{"
	for _, bucket := range h.buckets {
		for _, entry := range bucket {
			result += fmt.Sprintf("%s: %d, ", entry.key, entry.value)
		}
	}
	return result + "}"
}

func main() {
	hashMap := NewHashMap(16, 0.75)
	hashMap.Put("apple", 1)
	hashMap.Put("banana", 2)
	hashMap.Put("orange", 3)

	fmt.Println(hashMap.Get("banana")) // Output: 2, true
	hashMap.Remove("banana")
	fmt.Println(hashMap.Get("banana")) // Output: 0, false
	fmt.Println(hashMap)               // Debugging view
}
```

---

### Implementation of HashSet in Go

```go
package main

import (
	"fmt"
)

// HashSet is a custom hashset implementation
type HashSet struct {
	buckets    [][]string
	capacity   int
	size       int
	loadFactor float64
}

// NewHashSet creates a new HashSet
func NewHashSet(initialCapacity int, loadFactor float64) *HashSet {
	return &HashSet{
		buckets:    make([][]string, initialCapacity),
		capacity:   initialCapacity,
		loadFactor: loadFactor,
	}
}

// hash computes the bucket index for a given key
func (h *HashSet) hash(key string) int {
	return int(hashString(key)) % h.capacity
}

// hashString is a simple hash function for strings
func hashString(key string) uint32 {
	hash := uint32(0)
	for _, c := range key {
		hash = 31*hash + uint32(c)
	}
	return hash
}

// Resize doubles the capacity and rehashes all keys
func (h *HashSet) resize() {
	oldBuckets := h.buckets
	h.capacity *= 2
	h.buckets = make([][]string, h.capacity)
	h.size = 0

	for _, bucket := range oldBuckets {
		for _, key := range bucket {
			h.Add(key)
		}
	}
}

// Add inserts a key into the set
func (h *HashSet) Add(key string) {
	if float64(h.size)/float64(h.capacity) > h.loadFactor {
		h.resize()
	}

	index := h.hash(key)
	bucket := h.buckets[index]

	// Check if the key already exists
	for _, existingKey := range bucket {
		if existingKey == key {
			return
		}
	}

	// Add the key
	h.buckets[index] = append(bucket, key)
	h.size++
}

// Contains checks if the set contains a key
func (h *HashSet) Contains(key string) bool {
	index := h.hash(key)
	bucket := h.buckets[index]

	for _, existingKey := range bucket {
		if existingKey == key {
			return true
		}
	}

	return false
}

// Remove deletes a key from the set
func (h *HashSet) Remove(key string) {
	index := h.hash(key)
	bucket := h.buckets[index]

	for i, existingKey := range bucket {
		if existingKey == key {
			h.buckets[index] = append(bucket[:i], bucket[i+1:]...)
			h.size--
			return
		}
	}
}

// String returns all keys as a string
func (h *HashSet) String() string {
	result := "{"
	for _, bucket := range h.buckets {
		for _, key := range bucket {
			result += fmt.Sprintf("%s, ", key)
		}
	}
	return result + "}"
}

func main() {
	hashSet := NewHashSet(16, 0.75)
	hashSet.Add("apple")
	hashSet.Add("banana")
	hashSet.Add("orange")

	fmt.Println(hashSet.Contains("banana")) // Output: true
	hashSet.Remove("banana")
	fmt.Println(hashSet.Contains("banana")) // Output: false
	fmt.Println(hashSet)                    // Debugging view
}
```

---

### Explanation of Code:

1. **Hash Function:**
    
    - The `hashString` function computes a simple hash for strings, ensuring a relatively uniform distribution.
2. **Buckets:**
    
    - Both HashMap and HashSet use an array of slices to store values. Each bucket corresponds to a hash index.
3. **Collision Handling:**
    
    - Collisions are resolved using chaining, where elements are appended to the bucket slice.
4. **Resizing:**
    
    - If the load factor (size/capacity) exceeds a threshold, the data structure resizes itself and redistributes elements.
5. **Operations:**
    
    - HashMap supports `Put`, `Get`, and `Remove` for key-value pairs.
    - HashSet supports `Add`, `Contains`, and `Remove` for unique keys.
6. **Complexity:**
    
    - Average Case: O(1)O(1) for insert, lookup, and delete.
    - Worst Case: O(N)O(N) when all elements hash to the same bucket.

This provides a solid foundation for understanding hashing and custom data structures in Go.