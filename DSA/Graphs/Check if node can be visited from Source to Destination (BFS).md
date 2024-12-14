**Definition:**  
BFS is a graph traversal algorithm that explores all the vertices at the current depth level before moving on to nodes at the next depth level.

---

**Steps to Implement BFS:**

1. **Create an adjacency list** for the graph.
2. Start with the **source node**, insert it into a queue, and mark it as visited.
3. While the queue is **not empty**:
    - Dequeue the front node (`u`) from the queue.
    - For each neighbor of `u`:
        - If the neighbor is the **destination**, return `True`.
        - Otherwise, if it hasn't been visited:
            - Mark it as visited.
            - Add it to the queue.
4. If the loop completes without finding the destination, return `False`.

---

### Example with Diagram

Consider a graph:  
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

**BFS from A to E:**

1. Start at A.
2. Explore neighbors B, C.
3. Explore B’s neighbor D.
4. Explore C’s neighbor E (destination found).

**Diagram:**

```
     A
    / \
   B   C
   |    \
   D     E
```

---

### Python Code

```python
from collections import deque

def bfs(graph, source, destination):
    visited = set()
    queue = deque([source])
    visited.add(source)

    while queue:
        u = queue.popleft()

        if u == destination:
            return True
        
        for neighbor in graph[u]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return False

# Example usage
graph = {
    'A': ['B', 'C'],
    'B': ['D'],
    'C': ['E'],
    'D': [],
    'E': []
}

print(bfs(graph, 'A', 'E'))  # Output: True
```

---

### Go Code

```go
package main

import (
	"container/list"
	"fmt"
)

func bfs(graph map[string][]string, source, destination string) bool {
	visited := make(map[string]bool)
	queue := list.New()

	queue.PushBack(source)
	visited[source] = true

	for queue.Len() > 0 {
		element := queue.Front()
		u := element.Value.(string)
		queue.Remove(element)

		if u == destination {
			return true
		}

		for _, neighbor := range graph[u] {
			if !visited[neighbor] {
				visited[neighbor] = true
				queue.PushBack(neighbor)
			}
		}
	}

	return false
}

func main() {
	graph := map[string][]string{
		"A": {"B", "C"},
		"B": {"D"},
		"C": {"E"},
		"D": {},
		"E": {},
	}

	fmt.Println(bfs(graph, "A", "E")) // Output: true
}
```

This approach ensures BFS is easy to recall, with clear steps, a visual representation, and examples in Python and Go.