## **Concept**

To compute the sum of the **maximum element** for all subsequences of an array:

1. **Observation**:  
    Each element in the array contributes to being the maximum in multiple subsequences.
2. The key insight is:
    - If an element `arr[i]` is chosen as the **maximum** in a subsequence, it will include all subsequences where `arr[i]` is the largest among elements.
    - The number of subsequences where `arr[i]` is the maximum = 2count of elements to the left×2count of elements to the right2^{\text{count of elements to the left}} \times 2^{\text{count of elements to the right}}.
    - This counts all possible subsequences **including `arr[i]` and making it the maximum**.

To simplify:

- Sort the array.
- Use a combinatorial approach to compute the contribution of each element to the total sum.

---
### **Total Subsequences**

The total number of subsequences for an array of size `n` is 2n2^n2n.  
For `n = 3`, we have 23=82^3 = 823=8 subsequences, including the empty subsequence:

- `[]`
- `[3]`
- `[1]`
- `[2]`
- `[3, 1]`
- `[3, 2]`
- `[1, 2]`
- `[3, 1, 2]`
## **Intuition**

1. For each element in a **sorted array**:
    - Compute its contribution as the maximum in subsequences.
2. Use the fact that:
    - There are 2i2^{i} ways to pick elements **before** the current element.
    - There are 2n−i−12^{n-i-1} ways to pick elements **after** the current element.

### Contribution Formula:

The contribution of `arr[i]` in sorted order is:

Contribution=arr[i]×2i\text{Contribution} = \text{arr[i]} \times 2^{i}

Where ii is the index of the element.

---

## **Steps**

1. **Sort the array** in ascending order.
2. For each element at index `i`, calculate:
    - Contribution = `arr[i] * (2^i)`
3. Sum up the contributions of all elements.

---

## **Example**

Given array: `[3, 1, 2]`

1. **Sort the array**: `[1, 2, 3]`
    
2. **Calculate contributions**:
    
    - Element `1` at index `0`: 1×20=11 \times 2^0 = 1
    - Element `2` at index `1`: 2×21=42 \times 2^1 = 4
    - Element `3` at index `2`: 3×22=123 \times 2^2 = 12
3. **Sum of contributions**: 1+4+12=171 + 4 + 12 = 17
    

---

## **Python Code**

```python
def sum_of_maximums(arr):
    arr.sort()  # Sort the array
    n = len(arr)
    total_sum = 0
    
    for i in range(n):
        contribution = arr[i] * (1 << i)  # 2^i using bit shift
        total_sum += contribution

    return total_sum

# Example Usage
arr = [3, 1, 2]
result = sum_of_maximums(arr)
print("Sum of maximum of every subsequence:", result)
```

### **Output**:

```
Sum of maximum of every subsequence: 17
```

---

## **Go Code**

```go
package main

import (
	"fmt"
	"sort"
)

func sumOfMaximums(arr []int) int {
	sort.Ints(arr) // Sort the array
	totalSum := 0
	n := len(arr)

	for i := 0; i < n; i++ {
		contribution := arr[i] * (1 << i) // 2^i using bit shift
		totalSum += contribution
	}

	return totalSum
}

func main() {
	arr := []int{3, 1, 2}
	result := sumOfMaximums(arr)
	fmt.Println("Sum of maximum of every subsequence:", result)
}
```

### **Output**:

```
Sum of maximum of every subsequence: 17
```

---

## **Key Points to Remember**

1. **Sort the array** first.
2. Use the fact that an element at position `i` contributes:  
    arr[i]×2i\text{arr[i]} \times 2^i.
3. Sum all contributions to get the final result.

### **Time Complexity**: O(nlog⁡n)O(n \log n)

- Sorting the array takes O(nlog⁡n)O(n \log n), and calculating contributions takes O(n)O(n).

### **Space Complexity**: O(1)O(1)

- Constant space is used.