**Concept:**  
DFS explores a graph by starting at the source node and going as deep as possible along each branch before backtracking. It uses recursion or a stack for traversal.

---

### Steps:

1. **Initialize Adjacency List:** Represent the graph as an adjacency list.
2. **Mark Nodes as Visited:** Use a `visited` array to track visited nodes.
3. **Recursive/Iterative DFS:**
    - Start at the source node.
    - Visit all connected nodes recursively or iteratively using a stack.
4. **Connected Components:**
    - Loop through all nodes.
    - For any unvisited node, start a DFS and increment the connected components counter.
5. Return the count of connected components if needed.

---

### Key Use Cases:

1. Graph traversal.
2. Checking connectivity of nodes.
3. Counting connected components in a graph.

---

### Example with Diagram

**Graph Representation:**  
Adjacency List:

```
0 -> [1, 2]
1 -> [0, 3]
2 -> [0]
3 -> [1]
4 -> [5]
5 -> [4]
```

Graph with 2 connected components:

```
0 - 1 - 3      4 - 5
 \ 
  2
```

**DFS Traversal (starting from 0):**  
`0 -> 1 -> 3 -> 2`

**Connected Components Count:**  
`2`

---

### Python Code

```python
def dfs(node, adj_list, visited):
    visited[node] = True
    for neighbor in adj_list[node]:
        if not visited[neighbor]:
            dfs(neighbor, adj_list, visited)

def count_connected_components(n, edges):
    # Create adjacency list
    adj_list = {i: [] for i in range(n)}
    for u, v in edges:
        adj_list[u].append(v)
        adj_list[v].append(u)

    visited = [False] * n
    components = 0

    for i in range(n):
        if not visited[i]:  # Start DFS from unvisited node
            components += 1
            dfs(i, adj_list, visited)

    return components

# Example usage
n = 6  # Number of nodes
edges = [[0, 1], [0, 2], [1, 3], [4, 5]]  # Graph edges
print(count_connected_components(n, edges))  # Output: 2
```

---

### Go Code

```go
package main

import "fmt"

func dfs(node int, adjList map[int][]int, visited []bool) {
	visited[node] = true
	for _, neighbor := range adjList[node] {
		if !visited[neighbor] {
			dfs(neighbor, adjList, visited)
		}
	}
}

func countConnectedComponents(n int, edges [][]int) int {
	// Create adjacency list
	adjList := make(map[int][]int)
	for _, edge := range edges {
		u, v := edge[0], edge[1]
		adjList[u] = append(adjList[u], v)
		adjList[v] = append(adjList[v], u)
	}

	visited := make([]bool, n)
	components := 0

	for i := 0; i < n; i++ {
		if !visited[i] {
			components++
			dfs(i, adjList, visited)
		}
	}

	return components
}

func main() {
	n := 6
	edges := [][]int{{0, 1}, {0, 2}, {1, 3}, {4, 5}}
	fmt.Println(countConnectedComponents(n, edges)) // Output: 2
}
```

---

### Key Points to Remember:

1. **DFS Traversal Order:** Depends on adjacency list and graph structure.
2. **Connected Components:** Use DFS from each unvisited node to count components.
3. **Recursive vs Iterative:** Recursive DFS is easier to implement but may cause stack overflow for large graphs.
4. **Time Complexity:**
    - Graph represented as adjacency list: O(V+E)O(V + E), where VV is vertices and EE is edges.
5. **Space Complexity:** O(V)O(V) for `visited` array and recursion stack.