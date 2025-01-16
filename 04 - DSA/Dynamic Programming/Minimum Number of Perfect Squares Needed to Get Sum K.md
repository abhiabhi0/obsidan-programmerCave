This is a problem that can be solved using **Dynamic Programming (DP)**. The goal is to determine the minimum number of perfect squares that sum to KK.

---

### **Problem Breakdown**

1. A **perfect square** is a number like 1,4,9,16,…1, 4, 9, 16, \ldots, which is the square of an integer.
2. To find the minimum number of perfect squares that sum to KK, the idea is:
    - Start with KK.
    - For each i2i^2 (where i2≤Ki^2 \leq K), check if K−i2K - i^2 can be solved with fewer perfect squares.
3. Use a recursive relation: dp[j]=min⁡(dp[j−i2]+1)∀i2≤j\text{dp}[j] = \min(\text{dp}[j - i^2] + 1) \quad \forall i^2 \leq j Here:
    - dp[j]\text{dp}[j]: Minimum number of perfect squares to sum to jj.
    - dp[0]=0\text{dp}[0] = 0: Base case, no perfect squares needed to sum to 0.

---

### **Algorithm**

1. **Dynamic Programming Approach**:
    
    - Initialize a DP array where dp[j]\text{dp}[j] is set to infinity for all j≥1j \geq 1.
    - Iterate from 1 to KK and for each number jj, try all perfect squares i2i^2 where i2≤ji^2 \leq j.
    - Update dp[j]\text{dp}[j] as the minimum of its current value and dp[j−i2]+1\text{dp}[j - i^2] + 1.
2. **Optimizations**:
    
    - Precompute perfect squares i2i^2 up to KK.
    - Use a **bottom-up approach** to fill the DP array.

---

### **Python Implementation**

```python
import math

def minPerfectSquares(K):
    # Initialize dp array
    dp = [float('inf')] * (K + 1)
    dp[0] = 0  # Base case: 0 squares needed to sum to 0
    
    # Iterate through numbers from 1 to K
    for j in range(1, K + 1):
        # Try all perfect squares less than or equal to j
        for i in range(1, int(math.sqrt(j)) + 1):
            square = i * i
            dp[j] = min(dp[j], dp[j - square] + 1)
    
    return dp[K]

# Example Usage
print(minPerfectSquares(12))  # Output: 3 (12 = 4 + 4 + 4)
print(minPerfectSquares(13))  # Output: 2 (13 = 9 + 4)
```

---

### **Go Implementation**

```go
package main

import (
	"fmt"
	"math"
)

func minPerfectSquares(K int) int {
	// Initialize dp array
	dp := make([]int, K+1)
	for i := range dp {
		dp[i] = int(1e9) // Large number as infinity
	}
	dp[0] = 0 // Base case: 0 squares needed to sum to 0

	// Iterate through numbers from 1 to K
	for j := 1; j <= K; j++ {
		// Try all perfect squares less than or equal to j
		for i := 1; i*i <= j; i++ {
			square := i * i
			dp[j] = min(dp[j], dp[j-square]+1)
		}
	}
	return dp[K]
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}

func main() {
	fmt.Println(minPerfectSquares(12)) // Output: 3 (12 = 4 + 4 + 4)
	fmt.Println(minPerfectSquares(13)) // Output: 2 (13 = 9 + 4)
}
```

---

### **Complexity Analysis**

1. **Time Complexity**:
    
    - Outer loop: O(K)O(K), iterating through all values up to KK.
    - Inner loop: O(K)O(\sqrt{K}), iterating through all perfect squares up to KK.
    - Total: O(K⋅K)O(K \cdot \sqrt{K}).
2. **Space Complexity**:
    
    - O(K)O(K) for the DP array.

---

### **Example Walkthrough**

#### Input: K=12K = 12

- **Perfect Squares**: 1,4,91, 4, 9
- DP Table Update:
    - j=1j = 1: dp[1]=min⁡(dp[1−1]+1)=1\text{dp}[1] = \min(\text{dp}[1 - 1] + 1) = 1
    - j=2j = 2: dp[2]=min⁡(dp[2−1]+1)=2\text{dp}[2] = \min(\text{dp}[2 - 1] + 1) = 2
    - j=3j = 3: dp[3]=min⁡(dp[3−1]+1)=3\text{dp}[3] = \min(\text{dp}[3 - 1] + 1) = 3
    - j=4j = 4: dp[4]=min⁡(dp[4−1]+1,dp[4−4]+1)=1\text{dp}[4] = \min(\text{dp}[4 - 1] + 1, \text{dp}[4 - 4] + 1) = 1
    - j=5j = 5: dp[5]=min⁡(dp[5−1]+1,dp[5−4]+1)=2\text{dp}[5] = \min(\text{dp}[5 - 1] + 1, \text{dp}[5 - 4] + 1) = 2
    - ...
    - j=12j = 12: dp[12]=min⁡(dp[12−1]+1,dp[12−4]+1,dp[12−9]+1)=3\text{dp}[12] = \min(\text{dp}[12 - 1] + 1, \text{dp}[12 - 4] + 1, \text{dp}[12 - 9] + 1) = 3

#### Output: dp[12]=3\text{dp}[12] = 3

The result is **3**, as 12=4+4+412 = 4 + 4 + 4.