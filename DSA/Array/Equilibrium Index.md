The equilibrium index of an array is an index `i` such that the sum of all elements to the left of `i` is equal to the sum of all elements to the right of `i`.

---

### Algorithm:

1. Compute the **Prefix Sum (PS)** array:
    - `PS[i]` represents the sum of elements from the start to index `i` in the array.
2. Loop through the array from `1` to `N-2` (since indices `0` and `N-1` cannot be equilibrium indices):
    - At index `i`, check the condition: PS[i−1]==PS[N−1]−PS[i]\text{PS}[i-1] == \text{PS}[N-1] - \text{PS}[i] Here:
        - $\text{PS}[i-1]$: Sum of elements left to `i`.
        - $\text{PS}[N-1] - \text{PS}[i]$: Sum of elements right to `i`.

---

### Complexity:

- Time Complexity: $O(N)$
- Space Complexity: $O(N)$ (for the prefix sum array)

---

### Python Code

```python
def find_equilibrium_index(arr):
    n = len(arr)
    if n < 3:
        return -1  # No valid equilibrium index

    # Compute prefix sum array
    prefix_sum = [0] * n
    prefix_sum[0] = arr[0]
    for i in range(1, n):
        prefix_sum[i] = prefix_sum[i - 1] + arr[i]

    # Check equilibrium index
    for i in range(1, n - 1):
        left_sum = prefix_sum[i - 1]
        right_sum = prefix_sum[-1] - prefix_sum[i]
        if left_sum == right_sum:
            return i

    return -1  # No equilibrium index found
```

---

### Go Code

```go
package main

import "fmt"

func findEquilibriumIndex(arr []int) int {
    n := len(arr)
    if n < 3 {
        return -1 // No valid equilibrium index
    }

    // Compute prefix sum array
    prefixSum := make([]int, n)
    prefixSum[0] = arr[0]
    for i := 1; i < n; i++ {
        prefixSum[i] = prefixSum[i-1] + arr[i]
    }

    // Check equilibrium index
    for i := 1; i < n-1; i++ {
        leftSum := prefixSum[i-1]
        rightSum := prefixSum[n-1] - prefixSum[i]
        if leftSum == rightSum {
            return i
        }
    }

    return -1 // No equilibrium index found
}

func main() {
    arr := []int{-7, 1, 5, 2, -4, 3, 0}
    fmt.Println(findEquilibriumIndex(arr)) // Output: 3
}
```

---

### Example:

**Input:**  
`arr = [-7, 1, 5, 2, -4, 3, 0]`  
**Output:**  
`3` (Index 3 is the equilibrium index because the sums on both sides are equal: `[-7, 1, 5]` and `[-4, 3, 0]`)

---

### Key Points:

- Edge cases:
    - Array with less than 3 elements cannot have an equilibrium index.
    - Arrays with all equal elements can have multiple equilibrium indices.
- This solution assumes a single equilibrium index is returned.