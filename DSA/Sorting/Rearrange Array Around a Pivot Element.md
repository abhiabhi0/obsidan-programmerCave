**Objective:**  
Rearrange the array such that:

- The first element, $\text{arr}[0]$, moves to its sorted position.
- Elements to the left of $\text{arr}[0]$ are smaller.
- Elements to the right of arr[0]\text{arr}[0] are greater.

This is essentially a partition step of the Quick Sort algorithm.

---

### Algorithm Explanation:

1. **Partition Step:**
    
    - Initialize two pointers:
        - P1=1P1 = 1 (points to the first element after arr[0]\text{arr}[0]).
        - P2=n−1P2 = n-1 (points to the last element of the array).
    - **Loop until P1≤P2P1 \leq P2:**
        - Increment P1P1 until arr[P1]>arr[0]\text{arr}[P1] > \text{arr}[0].
        - Decrement P2P2 until arr[P2]<arr[0]\text{arr}[P2] < \text{arr}[0].
        - If P1≤P2P1 \leq P2:
            - Swap arr[P1]\text{arr}[P1] and arr[P2]\text{arr}[P2].
    - **Post-loop:**
        - P1P1 will point to the first element greater than arr[0]\text{arr}[0].
        - Swap arr[0]\text{arr}[0] and arr[P1−1]\text{arr}[P1-1].
2. **Final Arrangement:**
    
    - After the above steps, arr[0]\text{arr}[0] will be in its sorted position.
    - Elements to the left will be smaller, and elements to the right will be greater.
3. **Recursive Sorting:**
    
    - Recursively apply the same steps to the subarrays to the left and right of arr[0]\text{arr}[0] for a full sort (Quick Sort).

---

### Python Implementation:

```python
def rearrange(arr, left, right):
    pivot = arr[left]
    P1 = left + 1
    P2 = right

    while P1 <= P2:
        # Find the first element greater than pivot
        while P1 <= right and arr[P1] <= pivot:
            P1 += 1
        # Find the first element smaller than pivot
        while P2 >= left and arr[P2] > pivot:
            P2 -= 1
        # Swap elements at P1 and P2 if P1 <= P2
        if P1 <= P2:
            arr[P1], arr[P2] = arr[P2], arr[P1]
    
    # Place pivot in its correct position
    arr[left], arr[P2] = arr[P2], arr[left]
    return P2

def quick_sort(arr, left, right):
    if left < right:
        # Rearrange and get pivot index
        pivot_index = rearrange(arr, left, right)
        # Recursively sort left and right subarrays
        quick_sort(arr, left, pivot_index - 1)
        quick_sort(arr, pivot_index + 1, right)

# Example Usage
arr = [3, 6, 8, 10, 1, 2, 1]
print("Original Array:", arr)
quick_sort(arr, 0, len(arr) - 1)
print("Sorted Array:", arr)
```

---

### Go Implementation:

```go
package main

import "fmt"

// Function to rearrange array around the pivot
func rearrange(arr []int, left, right int) int {
    pivot := arr[left]
    P1 := left + 1
    P2 := right

    for P1 <= P2 {
        // Find the first element greater than pivot
        for P1 <= right && arr[P1] <= pivot {
            P1++
        }
        // Find the first element smaller than pivot
        for P2 >= left && arr[P2] > pivot {
            P2--
        }
        // Swap elements at P1 and P2 if P1 <= P2
        if P1 <= P2 {
            arr[P1], arr[P2] = arr[P2], arr[P1]
        }
    }

    // Place pivot in its correct position
    arr[left], arr[P2] = arr[P2], arr[left]
    return P2
}

// Quick Sort function
func quickSort(arr []int, left, right int) {
    if left < right {
        // Rearrange and get pivot index
        pivotIndex := rearrange(arr, left, right)
        // Recursively sort left and right subarrays
        quickSort(arr, left, pivotIndex-1)
        quickSort(arr, pivotIndex+1, right)
    }
}

func main() {
    arr := []int{3, 6, 8, 10, 1, 2, 1}
    fmt.Println("Original Array:", arr)
    quickSort(arr, 0, len(arr)-1)
    fmt.Println("Sorted Array:", arr)
}
```

---

### Example Walkthrough:

**Input:**  
Array: [3,6,8,10,1,2,1][3, 6, 8, 10, 1, 2, 1]  
Pivot: arr[0]=3\text{arr}[0] = 3.

1. **Initial Partition:**
    
    - P1=1P1 = 1, P2=6P2 = 6.
    - Swap arr[P1]\text{arr}[P1] and arr[P2]\text{arr}[P2].
    - Array: [3,1,8,10,1,2,6][3, 1, 8, 10, 1, 2, 6].
2. **Final Placement:**
    
    - Swap arr[0]\text{arr}[0] and arr[P2]\text{arr}[P2].
    - Array: [2,1,3,10,1,8,6][2, 1, 3, 10, 1, 8, 6].
3. **Recursive Steps:**
    
    - Apply the same steps on subarrays.

**Output:**  
Sorted Array: [1,1,2,3,6,8,10][1, 1, 2, 3, 6, 8, 10].

---

### Complexity:

1. **Time Complexity:**
    
    - **Best/Average Case:** O(nlog⁡n)O(n \log n).
    - **Worst Case (already sorted):** O(n2)O(n^2).
2. **Space Complexity:**
    
    - O(log⁡n)O(\log n) (recursive stack space).