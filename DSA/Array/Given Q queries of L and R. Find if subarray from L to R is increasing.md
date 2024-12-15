### Problem Statement:

Given $Q$ queries of $L$ and $R$, determine if the subarray from $L$ to $R$ in the given array is strictly non-decreasing (increasing or constant).

---

### Key Insights:

1. **Preprocessing**:
    
    - Create a new array `change` where:
        - `change[i] = 0` if `arr[i] \geq arr[i-1]` (non-decreasing),
        - `change[i] = 1` if `arr[i] < arr[i-1]` (decreasing).
    
    Example:  
    For `arr = [1, 4, 4, 7, 6, 8, 2, 10, 20, 21]`,  
    `change = [0, 0, 0, 0, 1, 0, 1, 0, 0, 0]`.
    
2. **Prefix Sum Array**:
    
    - Compute prefix sum of `change`:  
        `ps = [0, 0, 0, 0, 1, 1, 2, 2, 2, 2]`.
3. **Query Evaluation**:
    
    - For a subarray $[L, R]$, check if all values in `change` are 0 using: PS[R]−PS[L]=0\text{PS}[R] - \text{PS}[L] = 0  
        If true, the subarray is increasing; otherwise, it is not.

---

### Complexity:

- **Preprocessing**: $O(N)$
- **Query Evaluation**: $O(1)$ per query

---

### Python Code

```python
def preprocess(arr):
    n = len(arr)
    # Create the `change` array
    change = [0] * n
    for i in range(1, n):
        if arr[i] < arr[i - 1]:
            change[i] = 1

    # Create the prefix sum array
    prefix_sum = [0] * n
    prefix_sum[0] = change[0]
    for i in range(1, n):
        prefix_sum[i] = prefix_sum[i - 1] + change[i]

    return prefix_sum

def is_increasing_query(prefix_sum, l, r):
    if l == 0:
        return prefix_sum[r] == 0
    return prefix_sum[r] - prefix_sum[l - 1] == 0

# Example Usage
arr = [1, 4, 4, 7, 6, 8, 2, 10, 20, 21]
prefix_sum = preprocess(arr)

queries = [(0, 3), (3, 5), (7, 9)]
results = [is_increasing_query(prefix_sum, l, r) for l, r in queries]

print(results)  # Output: [True, False, True]
```

---

### Go Code

```go
package main

import "fmt"

// Preprocess the array to compute the `change` and `prefixSum` arrays
func preprocess(arr []int) []int {
	n := len(arr)
	change := make([]int, n)
	prefixSum := make([]int, n)

	// Create `change` array
	for i := 1; i < n; i++ {
		if arr[i] < arr[i-1] {
			change[i] = 1
		}
	}

	// Create `prefixSum` array
	prefixSum[0] = change[0]
	for i := 1; i < n; i++ {
		prefixSum[i] = prefixSum[i-1] + change[i]
	}

	return prefixSum
}

// Query whether the subarray [L, R] is increasing
func isIncreasingQuery(prefixSum []int, l, r int) bool {
	if l == 0 {
		return prefixSum[r] == 0
	}
	return prefixSum[r]-prefixSum[l-1] == 0
}

func main() {
	arr := []int{1, 4, 4, 7, 6, 8, 2, 10, 20, 21}
	prefixSum := preprocess(arr)

	queries := [][2]int{{0, 3}, {3, 5}, {7, 9}}
	results := make([]bool, len(queries))

	for i, query := range queries {
		l, r := query[0], query[1]
		results[i] = isIncreasingQuery(prefixSum, l, r)
	}

	fmt.Println(results) // Output: [true, false, true]
}
```

---

### Example Walkthrough:

**Input**:  
`arr = [1, 4, 4, 7, 6, 8, 2, 10, 20, 21]`  
`queries = [(0, 3), (3, 5), (7, 9)]`

**Step-by-Step**:

1. Compute `change`:  
    `change = [0, 0, 0, 0, 1, 0, 1, 0, 0, 0]`.
    
2. Compute `prefix_sum`:  
    `prefix_sum = [0, 0, 0, 0, 1, 1, 2, 2, 2, 2]`.
    
3. Evaluate queries:
    
    - Query $(0, 3)$: $\text{PS}[3] - \text{PS}[0-1] = \text{PS}[3] = 0$ → **True**.
    - Query $(3, 5)$: $\text{PS}[5] - \text{PS}[3-1] = \text{PS}[5] - \text{PS}[2] = 1 - 0 = 1$ → **False**.
    - Query $(7, 9)$: $\text{PS}[9] - \text{PS}[7-1] = \text{PS}[9] - \text{PS}[6] = 2 - 2 = 0$ → **True**.

**Output**:  
`[True, False, True]`.