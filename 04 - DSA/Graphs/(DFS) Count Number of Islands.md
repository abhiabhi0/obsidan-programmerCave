**Problem Statement:**  
Given a matrix of `1`s and `0`s:

- `1` represents land (part of an island).
- `0` represents water.  
    The task is to count the number of connected components of `1`s, i.e., the number of islands.

---

### Approach:

1. **Traversal:**
    - Traverse every cell in the matrix.
    - If the cell contains `1` and is not visited:
        - Increment the island counter.
        - Start a DFS to mark all cells in the same connected component as visited.
2. **DFS Implementation:**
    - Explore all four possible directions (up, down, left, right) from the current cell.
    - Only continue the DFS for valid neighbors that are `1` and not visited.

---

### Key Points:

1. Use a `visited` matrix to avoid revisiting cells.
2. Consider boundary conditions to ensure neighbors remain within the matrix.
3. Each DFS call explores and marks all cells of one island.

---

### Example with Diagram

**Matrix Input:**

```
1 1 0 0 0
1 1 0 0 1
0 0 0 1 1
0 0 0 0 0
1 0 1 0 1
```

**Steps:**

- Start DFS from unvisited `1`s:
    - First island: Explore `(0, 0)` and mark all connected `1`s.
    - Second island: Start DFS at `(1, 4)` and explore connected `1`s.
    - Repeat for remaining unvisited `1`s.

**Output (Number of Islands):** `5`

---

### Python Code

```python
def num_islands(matrix):
    rows, cols = len(matrix), len(matrix[0])
    visited = [[False for _ in range(cols)] for _ in range(rows)]

    def dfs(x, y):
        # Base case: Out of bounds or water or already visited
        if x < 0 or y < 0 or x >= rows or y >= cols or matrix[x][y] == 0 or visited[x][y]:
            return
        visited[x][y] = True
        # Explore all 4 directions
        dfs(x - 1, y)  # Up
        dfs(x + 1, y)  # Down
        dfs(x, y - 1)  # Left
        dfs(x, y + 1)  # Right

    islands = 0
    for i in range(rows):
        for j in range(cols):
            if matrix[i][j] == 1 and not visited[i][j]:
                islands += 1
                dfs(i, j)

    return islands

# Example usage
matrix = [
    [1, 1, 0, 0, 0],
    [1, 1, 0, 0, 1],
    [0, 0, 0, 1, 1],
    [0, 0, 0, 0, 0],
    [1, 0, 1, 0, 1]
]
print(num_islands(matrix))  # Output: 5
```

---

### Go Code

```go
package main

import "fmt"

func dfs(matrix [][]int, visited [][]bool, x, y int) {
	rows := len(matrix)
	cols := len(matrix[0])

	// Base case: Out of bounds, water, or already visited
	if x < 0 || y < 0 || x >= rows || y >= cols || matrix[x][y] == 0 || visited[x][y] {
		return
	}

	// Mark the cell as visited
	visited[x][y] = true

	// Explore all 4 directions
	dfs(matrix, visited, x-1, y) // Up
	dfs(matrix, visited, x+1, y) // Down
	dfs(matrix, visited, x, y-1) // Left
	dfs(matrix, visited, x, y+1) // Right
}

func numIslands(matrix [][]int) int {
	rows := len(matrix)
	cols := len(matrix[0])
	visited := make([][]bool, rows)
	for i := 0; i < rows; i++ {
		visited[i] = make([]bool, cols)
	}

	islands := 0
	for i := 0; i < rows; i++ {
		for j := 0; j < cols; j++ {
			if matrix[i][j] == 1 && !visited[i][j] {
				islands++
				dfs(matrix, visited, i, j)
			}
		}
	}

	return islands
}

func main() {
	matrix := [][]int{
		{1, 1, 0, 0, 0},
		{1, 1, 0, 0, 1},
		{0, 0, 0, 1, 1},
		{0, 0, 0, 0, 0},
		{1, 0, 1, 0, 1},
	}
	fmt.Println(numIslands(matrix)) // Output: 5
}
```

---

### Key Points to Remember:

1. **DFS Traversal:** Mark all connected `1`s for each island.
2. **Matrix Bounds Check:** Avoid index out-of-bounds errors.
3. **Visited Matrix:** Ensure no cell is processed more than once.
4. **Time Complexity:**
    - O(N×M)O(N \times M), where NN and MM are rows and columns, since every cell is visited once.
5. **Space Complexity:**
    - O(N×M)O(N \times M) for the `visited` matrix.
    - Recursive DFS may use stack space proportional to the largest connected component.