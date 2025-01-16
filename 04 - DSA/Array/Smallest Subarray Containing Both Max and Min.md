### Problem Statement:

Find the smallest subarray that contains both the maximum and minimum values of the given array. The subarray should:

1. Contain only one occurrence of the maximum and minimum.
2. Have the maximum and minimum at the corners of the subarray.

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfEyABY2-u6T_-U3cQOfQcypvA0DV3Fwg41Fl0saH7CtvTHj1jioZLKm6xvJSZUpusdVay8M3mJbMNBt4XmbiJUKS27j08V3c43mx6GL6TcdE2yq4CVgfwQwvGDyhHFohcT4v21pZPJeVIMAV_1W3JISw?key=5htWp-2xX_egQvU14bcQKA)**
---
### Algorithm:

1. **Find Max and Min**: Traverse the array to determine the maximum (`max_val`) and minimum (`min_val`) values.
2. **Track Last Seen Positions**:
    - Use `last_min` to store the most recent index of `min_val`.
    - Use `last_max` to store the most recent index of `max_val`.
3. **Iterate Through the Array**:
    - If the current element is `min_val`:
        - Update `last_min` with the current index.
        - If `last_max` is not empty, compute the subarray length: `abs(last_min - last_max) + 1`.
    - If the current element is `max_val`:
        - Update `last_max` with the current index.
        - If `last_min` is not empty, compute the subarray length: `abs(last_max - last_min) + 1`.
4. **Keep Track of Minimum Length**: Update the minimum length of subarray whenever a valid subarray is found.

---

### Complexity:

- **Time Complexity**: $O(N)$ (single traversal)
- **Space Complexity**: $O(1)$ (constant space)

---

### Python Code

```python
def smallest_subarray_with_min_and_max(arr):
    if not arr:
        return 0
    
    max_val = max(arr)
    min_val = min(arr)
    
    last_min = -1
    last_max = -1
    min_length = float('inf')
    
    for i, val in enumerate(arr):
        if val == min_val:
            last_min = i
            if last_max != -1:
                min_length = min(min_length, abs(last_min - last_max) + 1)
        if val == max_val:
            last_max = i
            if last_min != -1:
                min_length = min(min_length, abs(last_max - last_min) + 1)
    
    return min_length if min_length != float('inf') else 0

# Example Usage
arr = [1, 3, 2, 1, 4, 1, 2, 3, 4, 1]
print(smallest_subarray_with_min_and_max(arr))  # Output: 2
```

---

### Go Code

```go
package main

import (
	"fmt"
	"math"
)

func smallestSubarrayWithMinAndMax(arr []int) int {
	if len(arr) == 0 {
		return 0
	}

	// Find min and max
	minVal, maxVal := arr[0], arr[0]
	for _, val := range arr {
		if val > maxVal {
			maxVal = val
		}
		if val < minVal {
			minVal = val
		}
	}

	lastMin, lastMax := -1, -1
	minLength := math.MaxInt32

	for i, val := range arr {
		if val == minVal {
			lastMin = i
			if lastMax != -1 {
				minLength = min(minLength, abs(lastMin-lastMax)+1)
			}
		}
		if val == maxVal {
			lastMax = i
			if lastMin != -1 {
				minLength = min(minLength, abs(lastMax-lastMin)+1)
			}
		}
	}

	if minLength == math.MaxInt32 {
		return 0
	}
	return minLength
}

func abs(a int) int {
	if a < 0 {
		return -a
	}
	return a
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func main() {
	arr := []int{1, 3, 2, 1, 4, 1, 2, 3, 4, 1}
	fmt.Println(smallestSubarrayWithMinAndMax(arr)) // Output: 2
}
```

---

### Example:

**Input:**  
`arr = [1, 3, 2, 1, 4, 1, 2, 3, 4, 1]`  
**Output:**  
`2` (The subarray `[4, 1]` contains both the maximum `4` and minimum `1`.)

---

### Key Points:

1. The `last_min` and `last_max` variables ensure that we dynamically track the most recent positions of `min_val` and `max_val`.
2. The subarray length is calculated only when both `min_val` and `max_val` have been encountered.
3. The algorithm efficiently finds the result in a single pass through the array.