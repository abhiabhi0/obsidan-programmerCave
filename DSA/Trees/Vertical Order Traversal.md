### Problem

Perform a vertical order traversal of a binary tree:

- Nodes are grouped by their horizontal distance (HD) from the root node:
    - Left child: $\text{HD} - 1$
    - Right child: $\text{HD} + 1$
- Traverse from the **minimum HD** (leftmost) to the **maximum HD** (rightmost).
- If two nodes have the same HD:
    - They should be ordered by their level (top to bottom).
    - If levels are the same, order by value.

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcyWvlTXEja9chp5hobka-GG_BFIZjue5gmqdKJj3bg69HhrfpcP4TT3aUKC1bDxvCUFj2B1TBu321efl05fuJMSarj2h-AKV5uHFJK-Gl7S8g6EoXGoIgmOnv1PP2M1h4sSF6xgKkOlDiC3w1GrGg3E0s?key=5htWp-2xX_egQvU14bcQKA)**
### Approach

1. Use a **hashmap** to group nodes by their horizontal distance (HD).
2. Perform a **BFS traversal** to process nodes level by level:
    - Track both the node and its HD in the queue.
    - Insert each node's value into the hashmap for its HD.
3. Finally, sort the keys (HDs) of the hashmap and print the nodes in order.

**Time Complexity**: O(nlog⁡n)O(n \log n) (sorting the keys).  
**Space Complexity**: O(n)O(n) (hashmap and queue storage).

---

### Python Implementation

```python
from collections import defaultdict, deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def vertical_order_traversal(root: TreeNode) -> list[list[int]]:
    if not root:
        return []

    # Hashmap to store nodes by horizontal distance (HD)
    column_map = defaultdict(list)
    queue = deque([(root, 0)])  # (node, horizontal distance)

    while queue:
        node, hd = queue.popleft()
        column_map[hd].append(node.val)
        if node.left:
            queue.append((node.left, hd - 1))
        if node.right:
            queue.append((node.right, hd + 1))

    # Sort by horizontal distance and return the result
    return [column_map[key] for key in sorted(column_map.keys())]

# Example Usage
# Tree:        1
#            /   \
#           2     3
#          / \   / \
#         4   5 6   7
# Output: [[4], [2], [1, 5, 6], [3], [7]]
root = TreeNode(1, TreeNode(2, TreeNode(4), TreeNode(5)), TreeNode(3, TreeNode(6), TreeNode(7)))
print(vertical_order_traversal(root))
```

---

### Go Implementation

```go
package main

import (
	"fmt"
	"sort"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func verticalOrderTraversal(root *TreeNode) [][]int {
	if root == nil {
		return [][]int{}
	}

	columnMap := map[int][]int{} // Map to store nodes by horizontal distance
	type NodeHD struct {
		node *TreeNode
		hd   int
	}
	queue := []NodeHD{{root, 0}} // Queue for BFS with (node, horizontal distance)

	for len(queue) > 0 {
		curr := queue[0]
		queue = queue[1:]
		columnMap[curr.hd] = append(columnMap[curr.hd], curr.node.Val)
		if curr.node.Left != nil {
			queue = append(queue, NodeHD{curr.node.Left, curr.hd - 1})
		}
		if curr.node.Right != nil {
			queue = append(queue, NodeHD{curr.node.Right, curr.hd + 1})
		}
	}

	// Sort the keys and create the result
	keys := []int{}
	for k := range columnMap {
		keys = append(keys, k)
	}
	sort.Ints(keys)

	result := [][]int{}
	for _, k := range keys {
		result = append(result, columnMap[k])
	}
	return result
}

// Example Usage
func main() {
	root := &TreeNode{
		1,
		&TreeNode{2, &TreeNode{4, nil, nil}, &TreeNode{5, nil, nil}},
		&TreeNode{3, &TreeNode{6, nil, nil}, &TreeNode{7, nil, nil}},
	}
	fmt.Println(verticalOrderTraversal(root)) // Output: [[4], [2], [1, 5, 6], [3], [7]]
}
```

---

### Key Points

1. **Horizontal Distance (HD)**:
    - Left child: $HD−1$.
    - Right child: $HD+1$.
2. **Traversal**: Use BFS for level-wise traversal and maintain HD for each node.
3. **Sorting**: Sort the keys (HDs) to process columns from left to right.

Let me know if this needs further refinement!