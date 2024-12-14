**Concept:**  
To find the shortest path in a grid or matrix (with blocked cells), BFS ensures all possible paths are explored level by level. Use a queue to track positions and their current distance from the source.

---
**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdVJIhtadVo8PF_kHOUUMSPJjzcLqwUGnXboxvUpGHxjZQNaRZQQMBlSWUmVLNaKzrFH-Nb8f8H5Dv__v40OFluUN9_gKwdA1k6jEDeBz3tn3xgtSz7yjeVQt-SVZBn82UUsD5bcdmoRDCwwmv6sSgngF0-?key=5htWp-2xX_egQvU14bcQKA)**
**Steps to Implement:**

1. Use a queue to store the current cell coordinates and its distance from the source: `queue<pair<pair<int, int>, int>>`.
2. Maintain a **visited matrix** to ensure each cell is visited only once.
3. Use directional arrays `ROW = {-1, 0, 0, 1}` and `COL = {0, 1, -1, 0}` to traverse **up, down, left, and right**.
4. Initialize the source cell in the queue with distance 0 and mark it as visited.
5. While the queue is not empty:
    - Pop the front cell (`u`) and its distance.
    - Check if `u` is the destination. If yes, return the distance.
    - For each valid neighbor:
        - If the cell is unvisited and not blocked, mark it as visited and add it to the queue with distance incremented by 1.
6. If the destination is not reachable, return `-1`.

---

### Example with Diagram

**Matrix:**  
`1` represents open cells, and `0` represents blocked cells.

```
1 1 0 1
1 0 1 1
1 1 1 0
0 1 1 1
```

**Source:** (0, 0)  
**Destination:** (3, 3)

---

**Traversal Visualization:**  
BFS explores level by level:

- Start at (0, 0) with distance = 0.
- Explore neighbors (down, right) → Update distances.
- Continue until reaching (3, 3).

---

**Directional Arrays:**

- ROW = `{-1, 0, 0, 1}`
- COL = `{0, 1, -1, 0}`

**Diagram:**

```
Path: (0,0) → (1,0) → (2,1) → (3,3)
Shortest Distance = 5
```

---

### Python Code

```python
from collections import deque

def is_valid(matrix, visited, x, y):
    rows, cols = len(matrix), len(matrix[0])
    return 0 <= x < rows and 0 <= y < cols and not visited[x][y] and matrix[x][y] == 1

def shortest_path(matrix, source, destination):
    if not matrix or not matrix[source[0]][source[1]] or not matrix[destination[0]][destination[1]]:
        return -1

    rows, cols = len(matrix), len(matrix[0])
    visited = [[False] * cols for _ in range(rows)]
    
    queue = deque([((source[0], source[1]), 0)])  # ((x, y), distance)
    visited[source[0]][source[1]] = True

    # Directional arrays
    ROW = [-1, 0, 0, 1]
    COL = [0, 1, -1, 0]

    while queue:
        (x, y), dist = queue.popleft()

        if (x, y) == destination:
            return dist
        
        for i in range(4):  # Traverse 4 directions
            new_x, new_y = x + ROW[i], y + COL[i]
            if is_valid(matrix, visited, new_x, new_y):
                visited[new_x][new_y] = True
                queue.append(((new_x, new_y), dist + 1))

    return -1  # Destination not reachable

# Example usage
matrix = [
    [1, 1, 0, 1],
    [1, 0, 1, 1],
    [1, 1, 1, 0],
    [0, 1, 1, 1]
]
source = (0, 0)
destination = (3, 3)

print(shortest_path(matrix, source, destination))  # Output: 5
```

---

### Go Code

```go
package main

import (
	"container/list"
	"fmt"
)

type Point struct {
	x, y, dist int
}

func isValid(matrix [][]int, visited [][]bool, x, y int) bool {
	rows := len(matrix)
	cols := len(matrix[0])
	return x >= 0 && x < rows && y >= 0 && y < cols && !visited[x][y] && matrix[x][y] == 1
}

func shortestPath(matrix [][]int, source, destination [2]int) int {
	if len(matrix) == 0 || matrix[source[0]][source[1]] == 0 || matrix[destination[0]][destination[1]] == 0 {
		return -1
	}

	rows := len(matrix)
	cols := len(matrix[0])
	visited := make([][]bool, rows)
	for i := range visited {
		visited[i] = make([]bool, cols)
	}

	queue := list.New()
	queue.PushBack(Point{source[0], source[1], 0})
	visited[source[0]][source[1]] = true

	// Directional arrays
	ROW := []int{-1, 0, 0, 1}
	COL := []int{0, 1, -1, 0}

	for queue.Len() > 0 {
		element := queue.Front()
		queue.Remove(element)
		u := element.Value.(Point)

		if u.x == destination[0] && u.y == destination[1] {
			return u.dist
		}

		for i := 0; i < 4; i++ {
			new_x, new_y := u.x+ROW[i], u.y+COL[i]
			if isValid(matrix, visited, new_x, new_y) {
				visited[new_x][new_y] = true
				queue.PushBack(Point{new_x, new_y, u.dist + 1})
			}
		}
	}

	return -1 // Destination not reachable
}

func main() {
	matrix := [][]int{
		{1, 1, 0, 1},
		{1, 0, 1, 1},
		{1, 1, 1, 0},
		{0, 1, 1, 1},
	}
	source := [2]int{0, 0}
	destination := [2]int{3, 3}

	fmt.Println(shortestPath(matrix, source, destination)) // Output: 5
}
```

---

**Key Points to Remember:**

1. BFS is ideal for finding the shortest path in grids.
2. Use a queue to maintain `((x, y), distance)` pairs.
3. Directional arrays help simplify navigation to neighbors.
4. Maintain a `visited` matrix to prevent revisiting cells.