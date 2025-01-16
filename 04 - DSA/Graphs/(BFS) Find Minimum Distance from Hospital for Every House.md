**Concept:**  
In this problem, we simultaneously calculate the shortest distance of every house to its nearest hospital using **multi-source BFS**. By adding all hospitals to the queue initially, we can propagate distances to houses efficiently in a single traversal.

---
**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeamDNf4pijQ4bYbI2zMKymIDbkAYaaeIvzAKJxe_kMIwqcttTr9HUKZkoPBjEAC9brPxPYopjgwbCstIbYGm80ePkaeWagnfIQz2LQ6ZTSv_HGctOzyre8Ia8KuJpsVANT11mwPi-zU2_t8hd-KZIQeKBz?key=5htWp-2xX_egQvU14bcQKA)**

**Steps:**

1. **Initialization:**
    - Create a queue to store the coordinates of all hospitals with their distance (initially 0).
    - Mark all hospitals as visited.
2. **Multi-source BFS:**
    - While the queue is not empty:
        - Pop the front element (current cell and distance).
        - For each neighbor (up, down, left, right):
            - If the neighbor is a house and not visited:
                - Mark it as visited.
                - Update its distance as `current distance + 1`.
                - Add it to the queue.
3. Return the distance matrix, which now contains the minimum distance of each house from the nearest hospital.

---

### Example with Diagram

**Matrix Representation:**  
`1` represents a house, `2` represents a hospital, and `0` represents an empty space.

```
2 0 1 0
1 0 1 2
0 0 1 1
```

**Explanation:**

- Hospitals at `(0, 0)` and `(1, 3)` start with distance 0.
- Propagate distances to all reachable houses using BFS.

**Output (Distance Matrix):**

```
0 1 2 1
1 2 1 0
2 3 2 1
```

---

### Python Code

```python
from collections import deque

def min_distance_to_hospital(matrix):
    rows, cols = len(matrix), len(matrix[0])
    distances = [[-1] * cols for _ in range(rows)]  # Initialize distances as -1
    visited = [[False] * cols for _ in range(rows)]
    queue = deque()

    # Add all hospitals to the queue and mark as visited
    for i in range(rows):
        for j in range(cols):
            if matrix[i][j] == 2:  # Hospital
                queue.append((i, j, 0))  # (row, col, distance)
                visited[i][j] = True
                distances[i][j] = 0

    # Directional arrays
    ROW = [-1, 0, 0, 1]
    COL = [0, 1, -1, 0]

    # BFS
    while queue:
        x, y, dist = queue.popleft()

        for i in range(4):  # Traverse in 4 directions
            new_x, new_y = x + ROW[i], y + COL[i]

            if 0 <= new_x < rows and 0 <= new_y < cols and not visited[new_x][new_y]:
                if matrix[new_x][new_y] == 1:  # House
                    distances[new_x][new_y] = dist + 1
                    visited[new_x][new_y] = True
                    queue.append((new_x, new_y, dist + 1))

    return distances

# Example usage
matrix = [
    [2, 0, 1, 0],
    [1, 0, 1, 2],
    [0, 0, 1, 1]
]

result = min_distance_to_hospital(matrix)
for row in result:
    print(row)
# Output:
# [0, 1, 2, 1]
# [1, 2, 1, 0]
# [2, 3, 2, 1]
```

---

### Go Code

```go
package main

import (
	"container/list"
	"fmt"
)

type Cell struct {
	x, y, dist int
}

func minDistanceToHospital(matrix [][]int) [][]int {
	rows := len(matrix)
	cols := len(matrix[0])
	distances := make([][]int, rows)
	visited := make([][]bool, rows)
	queue := list.New()

	// Initialize distances and visited matrix
	for i := 0; i < rows; i++ {
		distances[i] = make([]int, cols)
		visited[i] = make([]bool, cols)
		for j := 0; j < cols; j++ {
			distances[i][j] = -1
			if matrix[i][j] == 2 { // Add hospitals to the queue
				queue.PushBack(Cell{i, j, 0})
				visited[i][j] = true
				distances[i][j] = 0
			}
		}
	}

	// Directional arrays
	ROW := []int{-1, 0, 0, 1}
	COL := []int{0, 1, -1, 0}

	// Multi-source BFS
	for queue.Len() > 0 {
		element := queue.Front()
		queue.Remove(element)
		cell := element.Value.(Cell)

		for i := 0; i < 4; i++ {
			new_x, new_y := cell.x+ROW[i], cell.y+COL[i]

			if new_x >= 0 && new_x < rows && new_y >= 0 && new_y < cols && !visited[new_x][new_y] {
				if matrix[new_x][new_y] == 1 { // House
					distances[new_x][new_y] = cell.dist + 1
					visited[new_x][new_y] = true
					queue.PushBack(Cell{new_x, new_y, cell.dist + 1})
				}
			}
		}
	}

	return distances
}

func main() {
	matrix := [][]int{
		{2, 0, 1, 0},
		{1, 0, 1, 2},
		{0, 0, 1, 1},
	}

	result := minDistanceToHospital(matrix)
	for _, row := range result {
		fmt.Println(row)
	}
	// Output:
	// [0 1 2 1]
	// [1 2 1 0]
	// [2 3 2 1]
}
```

---

**Key Points to Remember:**

1. Use **multi-source BFS** by adding all hospitals to the queue at the start.
2. Maintain a `visited` matrix to avoid reprocessing.
3. Track distances for each house using BFS propagation.
4. Time complexity: O(N×M)O(N \times M), where NN and MM are rows and columns.