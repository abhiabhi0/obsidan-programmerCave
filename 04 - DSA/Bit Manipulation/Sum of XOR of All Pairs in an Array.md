**Problem Statement:**  
Given an array of integers, calculate the sum of the XOR of all its pairs.

**Key Idea:**  
The XOR of two numbers has `1` at positions where their corresponding bits differ.  
For the entire array, we can efficiently calculate the sum of XORs by analyzing the contribution of each bit position.

---

### Approach:

1. **Contribution of Each Bit Position:**
    
    - For a given bit position `i`, calculate:
        - **count_ones:** Number of integers with the bit set (`1`) at position `i`.
        - **count_zeros:** Number of integers with the bit unset (`0`) at position `i`.
    - The number of differing pairs at bit position `i` is `count_ones * count_zeros`.  
        These pairs will contribute to the XOR sum at bit position `i`.
2. **Calculate Contribution to Final Sum:**
    
    - Multiply `count_ones * count_zeros` by \(2^i\) (the weight of bit position `i` in binary).
    - Since each pair `{a, b}` and `{b, a}` contributes the same value, multiply by `2`.
3. **Iterate Over All Bit Positions:**
    
    - Loop through 32 bit positions (for 32-bit integers).
    - Accumulate the contribution of all bit positions to get the final result.

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

- Position 0: `2 * (3 * 0) * \(2^0\) = 0`
- Position 1: `2 * (1 * 2) * \(2^1\) = 8`
- Position 2: `2 * (1 * 2) * \(2^2\) = 16`

**Final Sum:** `0 + 8 + 16 = 24`

---

### Python Code:

```python
def sum_of_xor_pairs(arr):
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
        # Add contribution of this bit position to the result
        result += 2 * count_ones * count_zeros * (1 << i)

    return result

# Example Usage:
arr = [5, 3, 1]
print("Sum of XOR of all pairs:", sum_of_xor_pairs(arr))
```

---

### Go Code:

```go
package main

import "fmt"

func sumOfXorPairs(arr []int) int {
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
        // Add contribution of this bit position to the result
        result += 2 * countOnes * countZeros * (1 << i)
    }

    return result
}

func main() {
    arr := []int{5, 3, 1}
    fmt.Println("Sum of XOR of all pairs:", sumOfXorPairs(arr))
}
```

---

### Time Complexity:

- **Outer Loop:** Runs for 32 bit positions → O(32)O(32).
- **Inner Loop:** Iterates through all elements in the array → O(N)O(N).
- **Overall Complexity:** O(32×N)=O(N)O(32 \times N) = O(N).

### Space Complexity:

- **Space Used:** O(1)O(1), as we use only constant extra space.

---

### Final Notes:

- This method avoids explicitly calculating XOR for all pairs, which would take O(N2)O(N^2) time.
- It efficiently calculates the sum of XORs by analyzing bit positions.