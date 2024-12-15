## **Concept**

- A **Complete Binary Tree (CBT)** is a binary tree where:
    1. All levels except the last are **completely filled**.
    2. Nodes in the last level are filled **from left to right**.

### **Key Idea**

- Use **Level Order Traversal** (Breadth-First Search).
- During traversal:
    1. If a **null node** is encountered, all subsequent nodes **must be null**.
    2. If a **non-null node** appears **after** a null node, the tree is not complete.

---

## **Steps**

1. Perform **Level Order Traversal** using a queue.
2. For each node:
    - If the node is **not null**, enqueue its **left** and **right** children.
    - If the node is **null**, mark it as "end" of valid nodes.
3. After encountering the first null node, if any **non-null** node is found, return `False`.
4. If traversal completes without violating the condition, return `True`.

---

## **Diagram**

Example of a **Complete Binary Tree**:

```
        1
       / \
      2   3
     / \  /
    4   5 6
```

Level Order Traversal: `1 → 2 → 3 → 4 → 5 → 6 → null → null → ...`

Non-Example (Not Complete Binary Tree):

```
        1
       / \
      2   3
         / \
        4   5
```

Here, `2` does not have a right child, and `3` still has children.

---

## **Python Code**

```python
from collections import deque

class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def isCompleteBinaryTree(root):
    if not root:
        return True

    queue = deque([root])
    end = False  # Flag to indicate if we encountered a null node

    while queue:
        node = queue.popleft()
        
        if node is None:
            end = True  # Encountered a null node
        else:
            if end:
                # If a non-null node is found after a null node
                return False
            
            queue.append(node.left)
            queue.append(node.right)
    
    return True

# Example Usage
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)
root.right.left = TreeNode(6)

print(isCompleteBinaryTree(root))  # Output: True
```

---

## **Go Code**

```go
package main

import (
	"fmt"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func isCompleteBinaryTree(root *TreeNode) bool {
	if root == nil {
		return true
	}

	queue := []*TreeNode{root}
	end := false

	for len(queue) > 0 {
		node := queue[0]
		queue = queue[1:]

		if node == nil {
			end = true // Null node encountered
		} else {
			if end {
				// If we find a non-null node after a null node
				return false
			}
			queue = append(queue, node.Left)
			queue = append(queue, node.Right)
		}
	}
	return true
}

func main() {
	root := &TreeNode{Val: 1}
	root.Left = &TreeNode{Val: 2}
	root.Right = &TreeNode{Val: 3}
	root.Left.Left = &TreeNode{Val: 4}
	root.Left.Right = &TreeNode{Val: 5}
	root.Right.Left = &TreeNode{Val: 6}

	fmt.Println(isCompleteBinaryTree(root)) // Output: true
}
```

---

## **Key Points to Remember**

1. Use **Level Order Traversal** to check completeness.
2. If a **null node** is encountered, all subsequent nodes in the traversal must also be null.
3. Use a **queue** to maintain the order of traversal.

### **Time Complexity**: O(N)

- Traverse all `N` nodes in the tree.

### **Space Complexity**: O(N)

- Queue stores up to `N` nodes in the worst case.