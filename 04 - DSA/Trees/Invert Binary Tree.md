### Problem

Invert a binary tree by swapping the left and right children of every node.

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeDzWhmPq6UsBMhkDWqPjsY6sePTPUjNp9fJncrhthpRi6J-A94A7kh4_18I0TrDIoevYrcWD1lWZoMEs7kpHA1Ccn9-8JT7YPM2c_CnwRJqg1MhVmVGhpgKj5CgnbBX14bta0Tht92i1a274JIr1WW3jI?key=5htWp-2xX_egQvU14bcQKA)**

### Approach

1. For a given root node:
    - Swap the left and right child nodes.
2. Recursively invert the left and right subtrees.

### Key Points

- Use **recursion** or **iteration** (via BFS or DFS) to traverse the tree.
- Swapping is performed at each node.

---

### Python Implementation

#### Recursive Solution

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def invert_tree(root: TreeNode) -> TreeNode:
    if not root:
        return None
    # Swap left and right children
    root.left, root.right = root.right, root.left
    # Recursively invert subtrees
    invert_tree(root.left)
    invert_tree(root.right)
    return root

# Example Usage
# Tree:      4
#          /   \
#         2     7
#        / \   / \
#       1   3 6   9
# After Inversion:
# Tree:      4
#          /   \
#         7     2
#        / \   / \
#       9   6 3   1
```

#### Iterative Solution (BFS)

```python
from collections import deque

def invert_tree_iterative(root: TreeNode) -> TreeNode:
    if not root:
        return None
    queue = deque([root])
    while queue:
        node = queue.popleft()
        # Swap left and right children
        node.left, node.right = node.right, node.left
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return root
```

---

### Go Implementation

#### Recursive Solution

```go
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }
    // Swap left and right children
    root.Left, root.Right = root.Right, root.Left
    // Recursively invert subtrees
    invertTree(root.Left)
    invertTree(root.Right)
    return root
}

// Example Usage
func main() {
    root := &TreeNode{4, &TreeNode{2, &TreeNode{1, nil, nil}, &TreeNode{3, nil, nil}}, &TreeNode{7, &TreeNode{6, nil, nil}, &TreeNode{9, nil, nil}}}
    fmt.Println(invertTree(root))
}
```

#### Iterative Solution (BFS)

```go
package main

import "container/list"

func invertTreeIterative(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }
    queue := list.New()
    queue.PushBack(root)
    for queue.Len() > 0 {
        node := queue.Remove(queue.Front()).(*TreeNode)
        // Swap left and right children
        node.Left, node.Right = node.Right, node.Left
        if node.Left != nil {
            queue.PushBack(node.Left)
        }
        if node.Right != nil {
            queue.PushBack(node.Right)
        }
    }
    return root
}
```

---

### Complexity

- **Time Complexity**: O(n)O(n), where nn is the number of nodes in the tree.
- **Space Complexity**:
    - O(h)O(h) for recursion, where hh is the height of the tree.
    - O(n)O(n) for iterative solution using BFS.

Let me know if further adjustments are needed!