## **Concept**

Given an array of size `n`, the goal is to compute the sum of all subset sums.

### Key Observation:

1. Each element in the array occurs in exactly **2n−12^{n-1}** subsets.
    
    - For each element, it either **includes** or **excludes** the element in a subset.
    - This creates 2n2^n total subsets, and every element is included in half of them → 2n−12^{n-1}.
2. Total contribution of an element `arr[i]` in all subsets = **arr[i] * 2n−12^{n-1}**.
    
3. Sum of all subset sums = Sum of contributions of all elements.
    

---

## **Steps**

1. Calculate the size of the array `n`.
2. For each element, multiply it by 2n−12^{n-1} (number of times it appears in all subsets).
3. Sum up these contributions to get the final result.

---

## **Example**

Given the array: `[1, 2, 3]`

- Total subsets: 23=82^3 = 8  
    Subsets: `[]`, `[1]`, `[2]`, `[3]`, `[1,2]`, `[1,3]`, `[2,3]`, `[1,2,3]`
    
- **Observation**:
    
    - `1` appears in 23−1=42^{3-1} = 4 subsets → Contribution = 1×4=41 \times 4 = 4
    - `2` appears in 44 subsets → Contribution = 2×4=82 \times 4 = 8
    - `3` appears in 44 subsets → Contribution = 3×4=123 \times 4 = 12
- **Sum of all subset sums**: 4+8+12=244 + 8 + 12 = 24
    

---

## **Python Code**

```python
def sum_of_all_subset_sums(arr):
    n = len(arr)
    total_sum = 0
    contribution_factor = 1 << (n - 1)  # 2^(n-1)
    
    for num in arr:
        total_sum += num * contribution_factor

    return total_sum

# Example Usage
arr = [1, 2, 3]
result = sum_of_all_subset_sums(arr)
print("Sum of all subset sums:", result)
```

### **Output**:

```
Sum of all subset sums: 24
```

---

## **Go Code**

```go
package main

import "fmt"

func sumOfAllSubsetSums(arr []int) int {
    n := len(arr)
    totalSum := 0
    contributionFactor := 1 << (n - 1) // 2^(n-1)

    for _, num := range arr {
        totalSum += num * contributionFactor
    }

    return totalSum
}

func main() {
    arr := []int{1, 2, 3}
    result := sumOfAllSubsetSums(arr)
    fmt.Println("Sum of all subset sums:", result)
}
```

### **Output**:

```
Sum of all subset sums: 24
```

---

## **Key Points to Remember**

1. **Each element occurs 2n−12^{n-1} times** in all subsets.
2. Contribution of each element = element×2n−1\text{element} \times 2^{n-1}.
3. Sum of all subset sums = **Sum of contributions of all elements**.

### **Time Complexity**: O(n)O(n)

- Only a single loop is required to compute contributions.

### **Space Complexity**: O(1)O(1)

- Constant space is used.