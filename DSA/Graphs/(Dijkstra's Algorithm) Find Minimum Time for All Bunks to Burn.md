**Problem Statement:** We need to find the minimum time after which all petrol bunks will burn. Given a graph with nodes representing petrol bunks and edges representing the time (weight) it takes to reach connected bunks, we need to compute the minimum time required to burn all bunks, starting from a source node.

We can solve this using **Dijkstra's Algorithm**, which finds the shortest path from a source node to all other nodes in a weighted graph.

---

### Steps:

1. **Initialize Distance Array:**
    
    - Create an array `distance[]` where each index represents a petrol bunk and the value at each index is the minimum time to reach that bunk from the source. Initially, set all values to infinity except for the source node, which will be 0.
2. **Min-Heap (Priority Queue):**
    
    - Use a **min-heap** to efficiently fetch the node with the minimum distance during each iteration.
3. **Dijkstra's Algorithm:**
    
    - Insert the source node with distance 0 into the min-heap.
    - Process nodes in the min-heap:
        - Pop the node with the smallest distance.
        - For each connected node, update the distance if a shorter path is found.
        - If the distance of a node is updated, insert it back into the min-heap.
4. **Result:**
    
    - After processing all nodes, the `distance[]` array will hold the minimum time to reach each petrol bunk. The result is the maximum value in this array, which represents the time when the last petrol bunk burns.

---

### Example:

**Input:**

- Graph with nodes representing petrol bunks and weighted edges representing time to travel.
- Source node is the starting bunk.

**Graph (Adjacency List):**

- `0: [(1, 4), (2, 2)]` // Node 0 connects to Node 1 with weight 4, Node 2 with weight 2.
- `1: [(0, 4), (3, 3)]`
- `2: [(0, 2), (3, 1)]`
- `3: [(1, 3), (2, 1)]`

**Source Node:** 0

**Distance Array (initial):**  
`[∞, ∞, ∞, ∞]`

**Min-Heap Initialization:**  
`[(0, 0)]` (source node with distance 0)

---

### Steps for Dijkstra's Algorithm:

1. Start with source node (0) with distance 0:
    
    - Pop `(0, 0)` from the min-heap.
    - Update `distance[0] = 0`.
2. Check all neighbors of node 0:
    
    - For node 1: `distance[1] = min(∞, 0 + 4) = 4`, insert `(4, 1)` into the min-heap.
    - For node 2: `distance[2] = min(∞, 0 + 2) = 2`, insert `(2, 2)` into the min-heap.
3. Pop `(2, 2)` from the min-heap:
    
    - Update `distance[2] = 2`.
4. Check all neighbors of node 2:
    
    - For node 0: Already processed.
    - For node 3: `distance[3] = min(∞, 2 + 1) = 3`, insert `(3, 3)` into the min-heap.
5. Pop `(3, 3)` from the min-heap:
    
    - Update `distance[3] = 3`.
6. Check all neighbors of node 3:
    
    - For node 1: `distance[1] = min(4, 3 + 3) = 4` (no update).
    - For node 2: Already processed.
7. Pop `(4, 1)` from the min-heap:
    
    - Update `distance[1] = 4`.
8. No further updates.
    

**Final Distance Array:**  
`[0, 4, 2, 3]`

**Result:** The maximum value in the distance array is 4, so the minimum time after which all bunks will be burned is **4**.

---

### Python Code:

```python
import heapq

def dijkstra(graph, source, n):
    # Distance array, initialized to infinity
    distance = [float('inf')] * n
    distance[source] = 0
    
    # Min-heap to store the node and its distance
    min_heap = [(0, source)]  # (distance, node)
    
    while min_heap:
        dist, node = heapq.heappop(min_heap)
        
        # If this distance is not the current known minimum, skip it
        if dist > distance[node]:
            continue
        
        # Explore all neighbors of the current node
        for neighbor, weight in graph[node]:
            new_dist = dist + weight
            if new_dist < distance[neighbor]:
                distance[neighbor] = new_dist
                heapq.heappush(min_heap, (new_dist, neighbor))
    
    # Return the maximum time from the source to all nodes
    return max(distance)

# Example Usage
graph = {
    0: [(1, 4), (2, 2)],
    1: [(0, 4), (3, 3)],
    2: [(0, 2), (3, 1)],
    3: [(1, 3), (2, 1)]
}
source = 0
n = 4  # Total number of nodes
print(dijkstra(graph, source, n))  # Output: 4
```

---

### Go Code:

```go
package main

import (
	"container/heap"
	"fmt"
)

// Define the priority queue
type Item struct {
	node     int
	distance int
}

type MinHeap []Item

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].distance < h[j].distance }
func (h MinHeap) Swap(i, j int)     { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(Item))
}

func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	item := old[n-1]
	*h = old[0 : n-1]
	return item
}

func dijkstra(graph map[int][][]int, source, n int) int {
	// Distance array, initialized to infinity
	distance := make([]int, n)
	for i := range distance {
		distance[i] = int(1e9)
	}
	distance[source] = 0
	
	// Min-heap to store nodes and their distances
	minHeap := &MinHeap{{node: source, distance: 0}}
	heap.Init(minHeap)
	
	for minHeap.Len() > 0 {
		// Get the node with the smallest distance
		item := heap.Pop(minHeap).(Item)
		node := item.node
		dist := item.distance
		
		// If the distance is already greater, skip it
		if dist > distance[node] {
			continue
		}
		
		// Explore neighbors
		for _, neighbor := range graph[node] {
			newDist := dist + neighbor[1]
			if newDist < distance[neighbor[0]] {
				distance[neighbor[0]] = newDist
				heap.Push(minHeap, Item{node: neighbor[0], distance: newDist})
			}
		}
	}
	
	// Find the maximum distance
	maxTime := 0
	for _, d := range distance {
		if d > maxTime {
			maxTime = d
		}
	}
	return maxTime
}

func main() {
	graph := map[int][][]int{
		0: {{1, 4}, {2, 2}},
		1: {{0, 4}, {3, 3}},
		2: {{0, 2}, {3, 1}},
		3: {{1, 3}, {2, 1}},
	}
	source := 0
	n := 4
	fmt.Println(dijkstra(graph, source, n))  // Output: 4
}
```

---

### Time Complexity:

- **Dijkstra's Algorithm:** O((N+E)log⁡N)O((N + E) \log N), where:
    - NN is the number of nodes (petrol bunks).
    - EE is the number of edges (connections between bunks).
    - log⁡N\log N arises due to heap operations.

### Space Complexity:

- O(N+E)O(N + E) for the adjacency list and heap.