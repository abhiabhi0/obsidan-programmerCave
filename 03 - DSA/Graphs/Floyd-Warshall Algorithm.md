**Problem Statement:** The Floyd-Warshall Algorithm is used to find the shortest distance between every pair of nodes in a graph. It works for both directed and undirected graphs and assumes that there are no negative edge weights. If there is no edge between two nodes, their distance is considered infinite.

---

### Steps:

1. **Initialize the Distance Matrix:**
    
    - Create a distance matrix `A[][]` where `A[i][j]` represents the shortest distance from node `i` to node `j`. Initialize the distance to infinity for all pairs of nodes unless there is a direct edge between them, in which case, initialize the distance to the edge weight.
2. **Iterate Over All Possible Intermediate Nodes:**
    
    - The algorithm considers each node `k` one by one as an intermediate node and checks whether a shorter path exists from node `i` to node `j` through node `k`.
3. **Update the Distance Matrix:**
    
    - For each pair of nodes `i` and `j`, update the distance `A[i][j]` by checking if a shorter path exists through `k`. The update rule is:
        
        ```
        A[i][j] = min(A[i][j], A[i][k] + A[k][j])
        ```
        
    - This means if the distance from `i` to `j` via `k` is shorter than the direct distance from `i` to `j`, then update `A[i][j]` with the new shorter distance.
4. **Repeat for All Nodes:**
    
    - Repeat the above process for every node `k` from 1 to `n` (the total number of nodes in the graph). After processing all intermediate nodes, the matrix `A` will contain the shortest distances between all pairs of nodes.

---

### Example:

**Graph (Adjacency Matrix):**

```
   0   1   2   3
0 [0, 3, ∞, ∞]
1 [∞, 0, 1, ∞]
2 [∞, ∞, 0, 2]
3 [∞, ∞, ∞, 0]
```

- Node 0 to Node 1: Distance = 3
- Node 1 to Node 2: Distance = 1
- Node 2 to Node 3: Distance = 2

**Initial Matrix A:**

```
   0   1   2   3
0 [0, 3, ∞, ∞]
1 [∞, 0, 1, ∞]
2 [∞, ∞, 0, 2]
3 [∞, ∞, ∞, 0]
```

---

### Iteration:

1. **When k = 0:**
    
    - Update the matrix for all pairs `(i, j)` by checking if a shorter path exists through node 0.
    
    After this step, the updated matrix is:
    
    ```
    A[0][2] = min(A[0][2], A[0][0] + A[0][2]) = ∞ (no change)
    A[1][2] = min(A[1][2], A[1][0] + A[0][2]) = 4
    A[2][3] = min(A[2][3], A[2][0] + A[0][3]) = 5
    ```
    
2. **When k = 1:**
    
    - Update the matrix for all pairs `(i, j)` by checking if a shorter path exists through node 1.
3. **When k = 2:**
    
    - Update the matrix for all pairs `(i, j)` by checking if a shorter path exists through node 2.
4. **When k = 3:**
    
    - Update the matrix for all pairs `(i, j)` by checking if a shorter path exists through node 3.

---

### Final Matrix A (Shortest Distances Between Every Pair of Nodes):

```
   0   1   2   3
0 [0, 3, 4, 6]
1 [4, 0, 1, 3]
2 [5, 6, 0, 2]
3 [7, 8, 9, 0]
```

---

### Python Code:

```python
def floyd_warshall(graph, n):
    # Initialize distance matrix
    A = [[float('inf')] * n for _ in range(n)]
    
    # Fill the initial distances (direct edges)
    for i in range(n):
        for j in range(n):
            if i == j:
                A[i][j] = 0
            elif graph[i][j] != 0:
                A[i][j] = graph[i][j]
    
    # Update the distance matrix using Floyd-Warshall algorithm
    for k in range(n):
        for i in range(n):
            for j in range(n):
                A[i][j] = min(A[i][j], A[i][k] + A[k][j])
    
    return A

# Example Usage
graph = [
    [0, 3, 0, 0],
    [0, 0, 1, 0],
    [0, 0, 0, 2],
    [0, 0, 0, 0]
]
n = 4  # Number of nodes
result = floyd_warshall(graph, n)
for row in result:
    print(row)
```

---

### Go Code:

```go
package main

import "fmt"

func floydWarshall(graph [][]int, n int) [][]int {
	// Initialize distance matrix
	A := make([][]int, n)
	for i := 0; i < n; i++ {
		A[i] = make([]int, n)
		for j := 0; j < n; j++ {
			if i == j {
				A[i][j] = 0
			} else if graph[i][j] != 0 {
				A[i][j] = graph[i][j]
			} else {
				A[i][j] = int(1e9) // infinity
			}
		}
	}

	// Update distance matrix using Floyd-Warshall algorithm
	for k := 0; k < n; k++ {
		for i := 0; i < n; i++ {
			for j := 0; j < n; j++ {
				if A[i][j] > A[i][k]+A[k][j] {
					A[i][j] = A[i][k] + A[k][j]
				}
			}
		}
	}
	return A
}

func main() {
	graph := [][]int{
		{0, 3, 0, 0},
		{0, 0, 1, 0},
		{0, 0, 0, 2},
		{0, 0, 0, 0},
	}
	n := 4
	result := floydWarshall(graph, n)
	for _, row := range result {
		fmt.Println(row)
	}
}
```

---

### Time Complexity:

- **Time Complexity:** O(N3)O(N^3), where NN is the number of nodes. The three nested loops each run for NN iterations.

### Space Complexity:

- **Space Complexity:** O(N2)O(N^2) for storing the distance matrix.