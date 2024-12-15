## **Concept**

- **Preorder**: Root → Left → Right
- **Inorder**: Left → Root → Right
- Given:
    - **Preorder** array: First element is the **root**.
    - **Inorder** array:
        - Elements to the left of root → **Left Subtree**
        - Elements to the right of root → **Right Subtree**

### **Steps**

1. Use a **hashmap** to store indices of elements in the **Inorder** traversal for O(1) lookup.
2. Recursively build the tree:
    - `Root` = First element of Preorder.
    - Divide Inorder into **Left** and **Right** subtrees based on root index.
    - Use subtree size to split Preorder into Left and Right parts.

---

## **Diagram**

For `Preorder = [3, 9, 20, 15, 7]` and `Inorder = [9, 3, 15, 20, 7]`:

```
Step 1: Preorder[0] = 3 (Root)
Inorder -> Left: [9] | Root: 3 | Right: [15, 20, 7]

            3
           / \
         9    ?

Step 2: Build Left Subtree from Preorder[1] = 9
Inorder -> Left: [] | Root: 9 | Right: []

            3
           / \
         9    ?

Step 3: Build Right Subtree from Preorder[2:4] = [20, 15, 7]
Inorder -> Left: [15] | Root: 20 | Right: [7]

            3
           / \
         9    20
             /  \
           15    7
```

---

## **Python Code**

```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def buildTree(preorder, inorder):
    inorder_map = {val: idx for idx, val in enumerate(inorder)}
    
    def helper(pre_start, pre_end, in_start, in_end):
        if pre_start > pre_end or in_start > in_end:
            return None
        
        root_val = preorder[pre_start]
        root = TreeNode(root_val)
        root_index = inorder_map[root_val]
        
        left_size = root_index - in_start
        
        root.left = helper(pre_start + 1, pre_start + left_size, in_start, root_index - 1)
        root.right = helper(pre_start + left_size + 1, pre_end, root_index + 1, in_end)
        
        return root
    
    return helper(0, len(preorder) - 1, 0, len(inorder) - 1)
```

---

## **Go Code**

```go
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func buildTree(preorder []int, inorder []int) *TreeNode {
    inorderMap := make(map[int]int)
    for i, v := range inorder {
        inorderMap[v] = i
    }

    var helper func(preStart, preEnd, inStart, inEnd int) *TreeNode
    helper = func(preStart, preEnd, inStart, inEnd int) *TreeNode {
        if preStart > preEnd || inStart > inEnd {
            return nil
        }

        rootVal := preorder[preStart]
        root := &TreeNode{Val: rootVal}
        rootIndex := inorderMap[rootVal]

        leftSize := rootIndex - inStart

        root.Left = helper(preStart+1, preStart+leftSize, inStart, rootIndex-1)
        root.Right = helper(preStart+leftSize+1, preEnd, rootIndex+1, inEnd)

        return root
    }

    return helper(0, len(preorder)-1, 0, len(inorder)-1)
}

func main() {
    preorder := []int{3, 9, 20, 15, 7}
    inorder := []int{9, 3, 15, 20, 7}
    root := buildTree(preorder, inorder)
    fmt.Println("Root Value:", root.Val) // Output: Root Value: 3
}
```

---

## **Key Points to Remember**

1. **Root** of the tree → `Preorder[pre_start]`.
2. **Inorder**: Split around root to get Left and Right subtrees.
3. Use **hashmap** for fast index lookup in Inorder array.
4. Subtree sizes help calculate Preorder boundaries.

### **Time Complexity**: O(N)

- Building the hashmap: O(N)
- Recursively traversing Preorder and Inorder: O(N).

### **Space Complexity**: O(N)

- Hashmap + Recursive call stack (O(N) in worst case for skewed trees).