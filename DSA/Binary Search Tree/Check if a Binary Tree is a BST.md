## **Concept**

To determine if a binary tree is a BST:

1. For each node:
    - The left subtree must be a **BST** and all its values < root value.
    - The right subtree must be a **BST** and all its values > root value.
2. Propagate information upward:
    - **isBST**: Whether the subtree is a valid BST.
    - **min value**: Minimum value in the subtree.
    - **max value**: Maximum value in the subtree.

### **Key Idea**

- Use **postorder traversal** to check subtrees first.
- For each node, combine the results of its left and right subtrees.
- Validate the current node:
    - `node.val > left subtree max`
    - `node.val < right subtree min`
- Return updated min and max values with the BST status.

---

## **Steps**

1. Define a helper function that:
    - Checks the left and right subtrees recursively.
    - Returns a tuple: `(isBST, minValue, maxValue)`.
2. If any subtree is not a BST, propagate `False` upward.
3. Validate the current node using the min and max values of the subtrees.

---

## **Diagram**

Example of a valid BST:

```
        10
       /  \
      5    15
     / \     \
    2   7     20
```

For each node:

- At node `10`:
    - Left subtree (5) → min = 2, max = 7
    - Right subtree (15) → min = 15, max = 20
    - Condition: `7 < 10 < 15` → **True**.

Invalid BST Example:

```
        10
       /  \
      5    15
           /  
          12  
         /
        11  
```

- At node `15`:
    - Left child `12` breaks the BST property since `12 < 15` but is in the right subtree.

---

## **Python Code**

```python
import math

class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

class Result:
    def __init__(self, isBST, minValue, maxValue):
        self.isBST = isBST
        self.minValue = minValue
        self.maxValue = maxValue

def isBST(root):
    def helper(node):
        if not node:
            return Result(True, math.inf, -math.inf)  # Base case: empty subtree
        
        # Recursively check left and right subtrees
        left = helper(node.left)
        right = helper(node.right)
        
        # Check current node validity
        if left.isBST and right.isBST and left.maxValue < node.val < right.minValue:
            # Update min and max values for the current subtree
            return Result(True, min(left.minValue, node.val), max(right.maxValue, node.val))
        else:
            return Result(False, 0, 0)  # Not a BST
        
    return helper(root).isBST

# Example Usage
root = TreeNode(10)
root.left = TreeNode(5)
root.right = TreeNode(15)
root.left.left = TreeNode(2)
root.left.right = TreeNode(7)
root.right.right = TreeNode(20)

print(isBST(root))  # Output: True
```

---

## **Go Code**

```go
package main

import (
	"fmt"
	"math"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

type Result struct {
	isBST    bool
	minValue int
	maxValue int
}

func isBST(root *TreeNode) bool {
	helper := func(node *TreeNode) Result {
		if node == nil {
			return Result{true, math.MaxInt64, math.MinInt64} // Base case
		}

		// Check left and right subtrees
		left := helper(node.Left)
		right := helper(node.Right)

		// Validate current node
		if left.isBST && right.isBST && left.maxValue < node.Val && node.Val < right.minValue {
			return Result{
				isBST:    true,
				minValue: min(left.minValue, node.Val),
				maxValue: max(right.maxValue, node.Val),
			}
		}

		return Result{false, 0, 0}
	}

	result := helper(root)
	return result.isBST
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	root := &TreeNode{Val: 10}
	root.Left = &TreeNode{Val: 5}
	root.Right = &TreeNode{Val: 15}
	root.Left.Left = &TreeNode{Val: 2}
	root.Left.Right = &TreeNode{Val: 7}
	root.Right.Right = &TreeNode{Val: 20}

	fmt.Println(isBST(root)) // Output: true
}
```

---

## **Key Points to Remember**

1. Use **postorder traversal** to process subtrees first.
2. Propagate:
    - Whether the subtree is a BST (`isBST`).
    - Minimum and maximum values in the subtree.
3. Validate the current node using left's `max` and right's `min`.
4. If any condition fails, propagate `False`.

### **Time Complexity**: O(N)

- Each node is visited once.

### **Space Complexity**: O(H)

- Recursive call stack, where `H` is the height of the tree.