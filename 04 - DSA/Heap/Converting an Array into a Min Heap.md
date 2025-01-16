**Key Points:**

1. A **Min Heap** is a binary tree where the root is the smallest element, and every parent is smaller than its children.
2. For an array, the heap property can be enforced using the **Heapify** algorithm.
3. **Heapify** works by fixing the heap property for a node and recursively ensuring its subtree also satisfies the heap property.
4. Build the heap by calling **Heapify** from the last non-leaf node to the root.

---

### Algorithm

1. Identify the last non-leaf node: index=⌊n2⌋−1\text{index} = \lfloor \frac{\text{n}}{2} \rfloor - 1, where n\text{n} is the size of the array.
2. Start from this index and move upwards, applying **Heapify** to each node.

---

### Example:

#### Input:

Array = [4, 6, 2, 3, 7, 5, 1, 8]

#### Step-by-step Transformation:

1. Start from the last non-leaf node (index=3\text{index} = 3).
2. Apply **Heapify** bottom-up.

#### Diagram of the transformation:

```
Initial Array: [4, 6, 2, 3, 7, 5, 1, 8]

Binary Tree Representation:
       4
    /    \
   6      2
  / \    / \
 3   7  5   1
/
8

Step 1: Fix node 3 (subtree rooted at 3):
       4
    /    \
   6      2
  / \    / \
 8   7  5   1  (3 swapped with 8)

Step 2: Fix node 2 (subtree rooted at 2):
       4
    /    \
   6      1
  / \    / \
 8   7  5   2  (2 swapped with 1)

Step 3: Fix node 1 (subtree rooted at 6):
       4
    /    \
   2      1
  / \    / \
 8   7  5   6  (6 swapped with 2)

Step 4: Fix root (subtree rooted at 4):
       1
    /    \
   2      4
  / \    / \
 8   7  5   6  (4 swapped with 1)
```

Final Min Heap Array: `[1, 2, 4, 8, 7, 5, 6, 3]`

---

### Python Code:

```python
def heapify(arr, n, i):
    smallest = i
    left = 2 * i + 1
    right = 2 * i + 2

    if left < n and arr[left] < arr[smallest]:
        smallest = left

    if right < n and arr[right] < arr[smallest]:
        smallest = right

    if smallest != i:
        arr[i], arr[smallest] = arr[smallest], arr[i]
        heapify(arr, n, smallest)

def build_min_heap(arr):
    n = len(arr)
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

# Example
array = [4, 6, 2, 3, 7, 5, 1, 8]
build_min_heap(array)
print("Min Heap:", array)
```

---

### Go Code:

```go
package main

import "fmt"

func heapify(arr []int, n, i int) {
    smallest := i
    left := 2*i + 1
    right := 2*i + 2

    if left < n && arr[left] < arr[smallest] {
        smallest = left
    }

    if right < n && arr[right] < arr[smallest] {
        smallest = right
    }

    if smallest != i {
        arr[i], arr[smallest] = arr[smallest], arr[i]
        heapify(arr, n, smallest)
    }
}

func buildMinHeap(arr []int) {
    n := len(arr)
    for i := n/2 - 1; i >= 0; i-- {
        heapify(arr, n, i)
    }
}

func main() {
    array := []int{4, 6, 2, 3, 7, 5, 1, 8}
    buildMinHeap(array)
    fmt.Println("Min Heap:", array)
}
```