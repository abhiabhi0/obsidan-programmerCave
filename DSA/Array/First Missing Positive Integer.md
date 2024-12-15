Given an unsorted integer array, find the first missing positive integer within the range $[1, N]$ (where $N$ is the size of the array). Solve it in $O(n)$ time and $O(1)$ space.

---

### Approach:

1. **Ignore Out-of-Range Elements**:
    
    - Replace all numbers $\leq 0$ or $> N$ with a placeholder value (e.g., $N+1$) since they are not relevant for the answer.
2. **Use Indexing to Track Presence**:
    
    - Treat the array indices as markers for the presence of numbers:
        - For each element $x$, mark the index $|x| - 1$ by negating the value at that index (if it's positive and within bounds).
3. **Identify the Missing Number**:
    
    - After marking, the first index $i$ where the value is positive indicates the missing number $i+1$.
4. **Edge Case**:
    
    - If all indices are marked (all values negative), the answer is $N+1$.

---

### Complexity:

- **Time**: $O(n)$ (single pass to preprocess, mark, and find the missing number)
- **Space**: $O(1)$ (in-place modification of the array)

---

### Python Code

```python
def first_missing_positive(nums):
    n = len(nums)

    # Step 1: Replace out-of-range numbers with N+1
    for i in range(n):
        if nums[i] <= 0 or nums[i] > n:
            nums[i] = n + 1

    # Step 2: Mark presence using indices
    for i in range(n):
        val = abs(nums[i])
        if 1 <= val <= n:
            nums[val - 1] = -abs(nums[val - 1])

    # Step 3: Find the first positive index
    for i in range(n):
        if nums[i] > 0:
            return i + 1

    # Step 4: All numbers are present, so return N+1
    return n + 1

# Example Usage
nums = [3, 4, -1, 1]
print(first_missing_positive(nums))  # Output: 2
```

---

### Go Code

```go
package main

import "fmt"

func firstMissingPositive(nums []int) int {
    n := len(nums)

    // Step 1: Replace out-of-range numbers with N+1
    for i := 0; i < n; i++ {
        if nums[i] <= 0 || nums[i] > n {
            nums[i] = n + 1
        }
    }

    // Step 2: Mark presence using indices
    for i := 0; i < n; i++ {
        val := abs(nums[i])
        if val >= 1 && val <= n {
            nums[val-1] = -abs(nums[val-1])
        }
    }

    // Step 3: Find the first positive index
    for i := 0; i < n; i++ {
        if nums[i] > 0 {
            return i + 1
        }
    }

    // Step 4: All numbers are present, so return N+1
    return n + 1
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}

func main() {
    nums := []int{3, 4, -1, 1}
    fmt.Println(firstMissingPositive(nums)) // Output: 2
}
```

---

### Explanation with Example:

**Input**:  
`nums = [3, 4, -1, 1]`

1. **Step 1**: Replace out-of-range values:  
    `nums = [3, 4, 5, 1]` (assuming 5 as $N+1$).
    
2. **Step 2**: Mark indices using presence:
    
    - For `nums[0] = 3`: Mark index $2 \rightarrow nums = [3, 4, -5, 1]$.
    - For `nums[1] = 4`: Mark index $3 \rightarrow nums = [3, 4, -5, -1]$.
    - For `nums[2] = 5`: Ignore (out of range).
    - For `nums[3] = 1`: Mark index $0 \rightarrow nums = [-3, 4, -5, -1]$.
3. **Step 3**: Find first positive value:
    
    - Index $1$ has value $4$, which is positive → Answer is $2$.

**Output**: `2`