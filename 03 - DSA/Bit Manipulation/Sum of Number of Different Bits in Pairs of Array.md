**Problem Statement:**  
Given an array of integers, we need to find the total number of differing bit positions in all pairs of numbers in the array.

**Key Idea:**  
For two integers, the number of differing bit positions can be calculated using XOR. The result of XOR has `1` at all differing bit positions.  
For the entire array, we can efficiently calculate this using bitwise operations.

---

### Approach:

1. **Understand the Contribution of Each Bit Position:**
    
    - For a bit position `i`:
        - Count the number of `1`s (`count_ones`) and `0`s (`count_zeros`) in that bit position for all numbers in the array.
        - The number of differing bits at position `i` is given by `count_ones * count_zeros` (each `1` pairs with each `0`).
        - Multiply this by `2` since each pair `{A, B}` is considered distinct from `{B, A}`.
2. **Iterate Over All Bit Positions:**
    
    - Loop through all 32 bit positions (for 32-bit integers).
    - For each bit position, calculate the number of differing bits using the above logic and add to the result.

---

### Algorithm:

1. Initialize `result = 0`.
2. For each bit position `i` from `0` to `31`:
    - Count the number of `1`s and `0`s in the array at bit position `i`.
    - Add `2 * count_ones * count_zeros` to `result`.
3. Return `result`.

---

### Example:

**Input Array:** `[5, 3, 1]`

- Binary Representation:
    
    ```
    5 -> 101
    3 -> 011
    1 -> 001
    ```
    
- At each bit position:
    - **Position 0:** `1 (5), 1 (3), 1 (1)` → `count_ones = 3`, `count_zeros = 0`
    - **Position 1:** `0 (5), 1 (3), 0 (1)` → `count_ones = 1`, `count_zeros = 2`
    - **Position 2:** `1 (5), 0 (3), 0 (1)` → `count_ones = 1`, `count_zeros = 2`

**Result Calculation:**

- Position 0: `2 * 3 * 0 = 0`
- Position 1: `2 * 1 * 2 = 4`
- Position 2: `2 * 1 * 2 = 4`

**Final Sum:** `0 + 4 + 4 = 8`

---

### Python Code:

```python
def sum_of_different_bits(arr):
    result = 0
    n = len(arr)

    # Loop through each bit position (0 to 31)
    for i in range(32):
        count_ones = 0

        # Count the number of 1s at the i-th bit position
        for num in arr:
            if (num >> i) & 1:
                count_ones += 1

        count_zeros = n - count_ones
        result += 2 * count_ones * count_zeros

    return result

# Example Usage:
arr = [5, 3, 1]
print("Sum of differing bits in pairs:", sum_of_different_bits(arr))
```

---

### Go Code:

```go
package main

import "fmt"

func sumOfDifferentBits(arr []int) int {
    result := 0
    n := len(arr)

    // Loop through each bit position (0 to 31)
    for i := 0; i < 32; i++ {
        countOnes := 0

        // Count the number of 1s at the i-th bit position
        for _, num := range arr {
            if (num>>i)&1 == 1 {
                countOnes++
            }
        }

        countZeros := n - countOnes
        result += 2 * countOnes * countZeros
    }

    return result
}

func main() {
    arr := []int{5, 3, 1}
    fmt.Println("Sum of differing bits in pairs:", sumOfDifferentBits(arr))
}
```

---

### Time Complexity:

- **Outer Loop:** Runs for 32 bit positions → O(32)O(32).
- **Inner Loop:** Iterates through all elements in the array for each bit position → O(N)O(N).
- **Overall Complexity:** O(32×N)=O(N)O(32 \times N) = O(N).

### Space Complexity:

- **Space Used:** O(1)O(1), as we use only constant extra space.

---

### Final Notes:

- This method is efficient for large arrays, as it avoids directly comparing every pair of numbers.
- The multiplication by `2` ensures both `{A, B}` and `{B, A}` are considered. If the problem treats these pairs as identical, remove the multiplication by `2`.