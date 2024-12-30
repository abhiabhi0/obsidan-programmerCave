**Concept:**  
The shortest path in an unweighted graph can be found using BFS. Along with each node in the queue, maintain the current distance from the source node.

---

**Steps:**

1. **Create an adjacency list** for the graph.
2. Start with the **source node**, insert it into a queue with a distance of 0, and mark it as visited.
3. While the queue is **not empty**:
    - Dequeue the front node (`u`) and its distance (`dist`).
    - If `u` is the **destination**, return `dist`.
    - For each neighbor of `u`:
        - If it hasn’t been visited:
            - Mark it as visited.
            - Add it to the queue with `dist + 1`.
4. If the loop completes without finding the destination, return `-1` (no path exists).

---

### Example with Diagram

Consider the same graph:  
Nodes: A, B, C, D, E  
Edges: A → B, A → C, B → D, C → E

**Adjacency List Representation:**

```
A: [B, C]
B: [D]
C: [E]
D: []
E: []
```

**Shortest Path from A to E:**

- Start at A with distance 0.
- Explore neighbors B, C (distances 1).
- Explore C’s neighbor E (distance 2 → destination found).

**Diagram:**

```
     A
    / \
   B   C
   |    \
   D     E
```

Shortest path: A → C → E (length = 2).

---

### Python Code

```python
from collections import deque

def find_min_distance(graph, source, destination):
    visited = set()
    queue = deque([(source, 0)])  # (node, distance)
    visited.add(source)

    while queue:
        u, dist = queue.popleft()

        if u == destination:
            return dist
        
        for neighbor in graph[u]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, dist + 1))
    
    return -1  # Destination not reachable

# Example usage
graph = {
    'A': ['B', 'C'],
    'B': ['D'],
    'C': ['E'],
    'D': [],
    'E': []
}

print(find_min_distance(graph, 'A', 'E'))  # Output: 2
```

---

### Go Code

```go
package main

import (
	"container/list"
	"fmt"
)

func findMinDistance(graph map[string][]string, source, destination string) int {
	visited := make(map[string]bool)
	queue := list.New()

	// Add (node, distance) to the queue
	queue.PushBack([2]interface{}{source, 0})
	visited[source] = true

	for queue.Len() > 0 {
		element := queue.Front()
		nodeDist := element.Value.([2]interface{})
		u := nodeDist[0].(string)
		dist := nodeDist[1].(int)
		queue.Remove(element)

		if u == destination {
			return dist
		}

		for _, neighbor := range graph[u] {
			if !visited[neighbor] {
				visited[neighbor] = true
				queue.PushBack([2]interface{}{neighbor, dist + 1})
			}
		}
	}

	return -1 // Destination not reachable
}

func main() {
	graph := map[string][]string{
		"A": {"B", "C"},
		"B": {"D"},
		"C": {"E"},
		"D": {},
		"E": {},
	}

	fmt.Println(findMinDistance(graph, "A", "E")) // Output: 2
}
```

---

**Key Points to Remember:**

1. BFS is optimal for finding the shortest path in an unweighted graph.
2. Maintain a queue of tuples: `(node, distance from source)`.
3. Stop as soon as the destination is reached.

This method ensures clarity and efficiency during interviews!