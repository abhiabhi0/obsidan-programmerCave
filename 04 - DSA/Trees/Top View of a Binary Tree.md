## **Concept**

- The **Top View** of a Binary Tree is the set of nodes visible when the tree is viewed from the top.
- From each vertical line, only the **first node** encountered at each level (leftmost or rightmost) is visible.
- **Level Order Traversal** is used to traverse nodes level by level.

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe7zh92ZotKY2kKakwtG1QybgyabYqJaIxpfZsPszeFVwVEkL7pC58tABOk33Zg6TT3CTjm099CN6Uo9oiIh0d6G5Kc2vLvXrepiOd8GwLFAT4LdTLW-0L3adnkFEpBBlndSJDex5hj8pMHw27cEHwXlw?key=5htWp-2xX_egQvU14bcQKA)**

---

### **Steps**

1. Use a **Queue** to perform Level Order Traversal.
    - Each queue element stores a **pair**: `(Node, Horizontal Distance (HD))`.
2. Use a **Map** to track the first node encountered at each horizontal distance (HD):
    - **Key**: Horizontal distance
    - **Value**: Node value
3. Process each node:
    - Pop the front node and its HD.
    - If HD is **not in the map**, store the node's value (first node encountered).
    - Push the **left child** with `HD - 1` and the **right child** with `HD + 1` into the queue.
4. Traverse the map (sorted by keys) to get the **Top View**.

---

## **Diagram**

Example:

```
        1
       / \
      2   3
     / \   \
    4   5   6
         \
          7
```

- Horizontal Distance (HD):
    
    ```
          0
         / \
       -1   +1
       / \     \
     -2   0     +2
           \
            +1
    ```
    

Top View: **[4, 2, 1, 3, 6]**

---

## **Python Code**

```python
from collections import deque

class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def topView(root):
    if not root:
        return []

    # Queue stores (Node, Horizontal Distance)
    queue = deque([(root, 0)])
    # Map to store first node at each horizontal distance
    hd_map = {}

    while queue:
        node, hd = queue.popleft()

        # Store the node if it's the first at its HD
        if hd not in hd_map:
            hd_map[hd] = node.val

        # Add children to the queue with updated HD
        if node.left:
            queue.append((node.left, hd - 1))
        if node.right:
            queue.append((node.right, hd + 1))

    # Sort the map by HD and return values
    return [hd_map[hd] for hd in sorted(hd_map.keys())]

# Example Usage
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)
root.right.right = TreeNode(6)
root.left.right.right = TreeNode(7)

print(topView(root))  # Output: [4, 2, 1, 3, 6]
```

---

## **Go Code**

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

func topView(root *TreeNode) []int {
	if root == nil {
		return []int{}
	}

	type Pair struct {
		Node *TreeNode
		HD   int
	}

	queue := []Pair{{Node: root, HD: 0}}
	hdMap := make(map[int]int)

	for len(queue) > 0 {
		// Pop from queue
		pair := queue[0]
		queue = queue[1:]

		node, hd := pair.Node, pair.HD

		// Store node if it's the first at this HD
		if _, exists := hdMap[hd]; !exists {
			hdMap[hd] = node.Val
		}

		// Push children into queue with updated HD
		if node.Left != nil {
			queue = append(queue, Pair{Node: node.Left, HD: hd - 1})
		}
		if node.Right != nil {
			queue = append(queue, Pair{Node: node.Right, HD: hd + 1})
		}
	}

	// Sort keys and extract values
	keys := []int{}
	for k := range hdMap {
		keys = append(keys, k)
	}
	sort.Ints(keys)

	result := []int{}
	for _, k := range keys {
		result = append(result, hdMap[k])
	}

	return result
}

func main() {
	root := &TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Right = &TreeNode{Val: 3}
	root.Left.Left = &TreeNode{Val: 4}
	root.Left.Right = &TreeNode{Val: 5}
	root.Right.Right = &TreeNode{Val: 6}
	root.Left.Right.Right = &TreeNode{Val: 7}

	fmt.Println(topView(root)) // Output: [4, 2, 1, 3, 6]
}
```

---

## **Key Points to Remember**

1. Use **Level Order Traversal** with Horizontal Distance (HD).
2. Store the **first node** at each HD in a **map**.
3. Use a queue to traverse nodes level by level and update HD.
4. Sort the map keys (HD) to get nodes from **leftmost** to **rightmost**.

### **Time Complexity**: O(N log N)

- Sorting the horizontal distances takes O(N log N).

### **Space Complexity**: O(N)

- Queue and map store all nodes in the tree.