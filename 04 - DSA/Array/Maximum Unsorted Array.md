Find the smallest subarray $\text{A[l], A[l+1], \dots, A[r]}$ such that sorting this subarray in ascending order will make the entire array sorted. If the array is already sorted, return `-1`.

---

### Approach:

1. **Initial Pointers**:
    
    - Find the first index $l$ from the left where $A[l] > A[l+1]$.
    - Find the first index $r$ from the right where $A[r] < A[r-1]$.
2. **Check Sorted Condition**:
    
    - If $l$ and $r$ are not found (i.e., the array is already sorted), return `-1`.
3. **Find Extremes in Subarray**:
    
    - Calculate the minimum and maximum values between $l$ and $r$.
4. **Expand Boundaries**:
    
    - Adjust $l$ to the left if any element in the range $[0, l-1]$ is greater than the minimum value.
    - Adjust $r$ to the right if any element in the range $[r+1, N-1]$ is smaller than the maximum value.
5. **Return the Indices**:
    
    - Return $[l, r]$ as the smallest subarray that, when sorted, makes the entire array sorted.

---

### Complexity:

- **Time**: $O(n)$ (single pass for finding $l$, $r$, min, max, and expanding boundaries)
- **Space**: $O(1)$ (in-place computations)

---

### Python Code

```python
def find_unsorted_subarray(arr):
    n = len(arr)
    l, r = -1, -1

    # Step 1: Find initial boundaries
    for i in range(n - 1):
        if arr[i] > arr[i + 1]:
            l = i
            break

    for j in range(n - 1, 0, -1):
        if arr[j] < arr[j - 1]:
            r = j
            break

    # If the array is already sorted
    if l == -1 or r == -1:
        return -1

    # Step 2: Find min and max in the subarray [l, r]
    subarray_min = min(arr[l:r + 1])
    subarray_max = max(arr[l:r + 1])

    # Step 3: Expand boundaries
    while l > 0 and arr[l - 1] > subarray_min:
        l -= 1
    while r < n - 1 and arr[r + 1] < subarray_max:
        r += 1

    return [l, r]

# Example Usage
arr = [1, 3, 2, 4, 5]
print(find_unsorted_subarray(arr))  # Output: [1, 2]
```

---

### Go Code

```go
package main

import (
	"fmt"
	"math"
)

func findUnsortedSubarray(arr []int) []int {
	n := len(arr)
	l, r := -1, -1

	// Step 1: Find initial boundaries
	for i := 0; i < n-1; i++ {
		if arr[i] > arr[i+1] {
			l = i
			break
		}
	}

	for j := n - 1; j > 0; j-- {
		if arr[j] < arr[j-1] {
			r = j
			break
		}
	}

	// If the array is already sorted
	if l == -1 || r == -1 {
		return []int{-1}
	}

	// Step 2: Find min and max in the subarray [l, r]
	subarrayMin, subarrayMax := math.MaxInt, math.MinInt
	for i := l; i <= r; i++ {
		if arr[i] < subarrayMin {
			subarrayMin = arr[i]
		}
		if arr[i] > subarrayMax {
			subarrayMax = arr[i]
		}
	}

	// Step 3: Expand boundaries
	for l > 0 && arr[l-1] > subarrayMin {
		l--
	}
	for r < n-1 && arr[r+1] < subarrayMax {
		r++
	}

	return []int{l, r}
}

func main() {
	arr := []int{1, 3, 2, 4, 5}
	fmt.Println(findUnsortedSubarray(arr)) // Output: [1, 2]
}
```

---

### Explanation with Example:

**Input**: `arr = [1, 3, 2, 4, 5]`

1. **Initial Boundaries**:
    
    - Starting from the left, find $l$: $A[1] > A[2]$, so $l = 1$.
    - Starting from the right, find $r$: $A[2] < A[1]$, so $r = 2$.
2. **Subarray Min and Max**:
    
    - Subarray: $[3, 2]$
    - $subarray_min = 2$, $subarray_max = 3$.
3. **Expand Boundaries**:
    
    - No element in $[0]$ is greater than $2$ → $l$ remains $1$.
    - No element in $[3, 4]$ is smaller than $3$ → $r$ remains $2$.

**Output**: `[1, 2]`