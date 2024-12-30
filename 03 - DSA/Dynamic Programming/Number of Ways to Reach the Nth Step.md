This is a classic problem in dynamic programming. It models the number of ways to climb a staircase where you can either take a single step or two steps at a time.

---

### **Problem Breakdown**

- Let f(k)f(k) represent the number of ways to reach the kk-th step.
    
- From the ii-th step:
    
    - You can go to (i+1)(i+1)-th step.
    - Or go to (i+2)(i+2)-th step.
- To reach the kk-th step, you could:
    
    1. Come from (k−1)(k-1)-th step (with f(k−1)f(k-1) ways).
    2. Come from (k−2)(k-2)-th step (with f(k−2)f(k-2) ways).

Thus, the recurrence relation is:

f(k)=f(k−1)+f(k−2)f(k) = f(k-1) + f(k-2)

---

### **Base Cases**

1. f(0)=1f(0) = 1: There is **1 way** to stay at the 0th step (do nothing).
2. f(1)=1f(1) = 1: There is **1 way** to reach the 1st step (take one step from 0).

---

### **Algorithm Approaches**

1. **Dynamic Programming (Bottom-Up)**:
    
    - Use an array `dp` to store the number of ways to reach each step.
    - Start from step 0 and iteratively compute the values for higher steps.
2. **Space Optimized DP**:
    
    - Since f(k)f(k) depends only on the last two values (f(k−1)f(k-1) and f(k−2)f(k-2)), we can reduce the space complexity to O(1)O(1).
3. **Recursive Approach with Memoization**:
    
    - Use recursion to calculate f(k)f(k), storing intermediate results to avoid redundant calculations.

---

### **Python Implementation**

#### **1. Dynamic Programming**

```python
def numWaysDP(n):
    # Handle edge cases
    if n == 0 or n == 1:
        return 1

    dp = [0] * (n + 1)
    dp[0], dp[1] = 1, 1

    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]

    return dp[n]

# Example Usage
print(numWaysDP(5))  # Output: 8
```

#### **2. Space Optimized**

```python
def numWaysOptimized(n):
    if n == 0 or n == 1:
        return 1

    prev1, prev2 = 1, 1

    for i in range(2, n + 1):
        curr = prev1 + prev2
        prev2 = prev1
        prev1 = curr

    return prev1

# Example Usage
print(numWaysOptimized(5))  # Output: 8
```

---

### **Go Implementation**

#### **1. Dynamic Programming**

```go
package main

import "fmt"

func numWaysDP(n int) int {
    // Handle edge cases
    if n == 0 || n == 1 {
        return 1
    }

    dp := make([]int, n+1)
    dp[0], dp[1] = 1, 1

    for i := 2; i <= n; i++ {
        dp[i] = dp[i-1] + dp[i-2]
    }

    return dp[n]
}

func main() {
    fmt.Println(numWaysDP(5)) // Output: 8
}
```

#### **2. Space Optimized**

```go
package main

import "fmt"

func numWaysOptimized(n int) int {
    if n == 0 || n == 1 {
        return 1
    }

    prev1, prev2 := 1, 1

    for i := 2; i <= n; i++ {
        curr := prev1 + prev2
        prev2 = prev1
        prev1 = curr
    }

    return prev1
}

func main() {
    fmt.Println(numWaysOptimized(5)) // Output: 8
}
```

---

### **Complexity Analysis**

1. **Time Complexity**:
    
    - Both approaches run in O(n)O(n), as we iterate through the range once.
2. **Space Complexity**:
    
    - **Dynamic Programming**: O(n)O(n), as we use a DP array of size n+1n+1.
    - **Space Optimized**: O(1)O(1), as only two variables are used to store the previous results.

---

### **Example Walkthrough**

#### Input: n=5n = 5

- f(0)=1f(0) = 1
- f(1)=1f(1) = 1
- f(2)=f(1)+f(0)=1+1=2f(2) = f(1) + f(0) = 1 + 1 = 2
- f(3)=f(2)+f(1)=2+1=3f(3) = f(2) + f(1) = 2 + 1 = 3
- f(4)=f(3)+f(2)=3+2=5f(4) = f(3) + f(2) = 3 + 2 = 5
- f(5)=f(4)+f(3)=5+3=8f(5) = f(4) + f(3) = 5 + 3 = 8

**Output**: 8 ways

**Explanation**:

1. [1, 1, 1, 1, 1]
2. [1, 1, 1, 2]
3. [1, 1, 2, 1]
4. [1, 2, 1, 1]
5. [2, 1, 1, 1]
6. [1, 2, 2]
7. [2, 1, 2]
8. [2, 2, 1]