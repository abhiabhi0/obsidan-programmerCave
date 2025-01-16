To solve this problem, we can use a **stack** data structure. The stack helps us keep track of elements while we iterate over the array. We maintain a stack of elements that are smaller than the current element. If the top of the stack is greater than the current element, we pop elements until we find one that is smaller. This way, we can efficiently find the nearest smaller element to the left for each element in the array.

### Steps:

1. **Initialize a Stack**: Start with an empty stack.
2. **Traverse the Array**: For each element:
    - If the stack is empty, the nearest smaller element is `-1` (since there is no element to the left).
    - If the stack's top is greater than the current element, pop elements from the stack until we find a smaller element.
    - The nearest smaller element will then be the top of the stack (or `-1` if the stack is empty).
3. **Push the Current Element to Stack**: After processing the current element, push it onto the stack to potentially be the nearest smaller number for future elements.

### Code Example (Python):

```python
def nearest_smaller_left(arr):
    n = len(arr)
    stack = []
    ans = []
    
    for i in range(n):
        # Pop elements from stack while top of stack is greater than or equal to current element
        while stack and stack[-1] >= arr[i]:
            stack.pop()
        
        # If stack is empty, there is no smaller element on the left
        if stack:
            ans.append(stack[-1])
        else:
            ans.append(-1)
        
        # Push current element to stack
        stack.append(arr[i])
    
    return ans

# Example Usage
arr = [4, 5, 2, 10, 8]
print(nearest_smaller_left(arr))  # Output: [-1, 4, -1, 2, 2]
```

### Explanation:

1. **Initialization**: We start by initializing an empty stack and an answer array `ans`.
2. **Iterating over Array**: For each element in the array:
    - We check the stack's top. If it's greater than or equal to the current element, we pop it off.
    - If the stack is not empty after popping, the nearest smaller element will be the top of the stack. Otherwise, it’s `-1` (no smaller element exists on the left).
    - Finally, we push the current element onto the stack.
3. **Returning the Result**: Once we’ve processed all the elements, the `ans` array contains the nearest smaller elements for each position.

### Time Complexity:

- **O(N)** where N is the number of elements in the array. Each element is pushed and popped from the stack at most once.

### Example Walkthrough:

For the input array `[4, 5, 2, 10, 8]`:

1. For `4`, the stack is empty, so the answer is `-1`. Stack becomes `[4]`.
2. For `5`, the stack top (`4`) is smaller than `5`, so the answer is `4`. Stack becomes `[4, 5]`.
3. For `2`, the stack top (`5`) is greater than `2`, so we pop `5`. Now, the stack top (`4`) is greater than `2`, so we pop `4`. The stack is empty, so the answer is `-1`. Stack becomes `[2]`.
4. For `10`, the stack top (`2`) is smaller than `10`, so the answer is `2`. Stack becomes `[2, 10]`.
5. For `8`, the stack top (`10`) is greater than `8`, so we pop `10`. The stack top (`2`) is smaller than `8`, so the answer is `2`. Stack becomes `[2, 8]`.

Output: `[-1, 4, -1, 2, 2]`

```go
package main

import "fmt"

func nearestSmallestToLeft(arr []int) []int {
    stack := []int{}
    result := make([]int, len(arr))
    
    for i, num := range arr {
        // Pop all elements from stack which are greater than or equal to current element
        for len(stack) > 0 && stack[len(stack)-1] >= num {
            stack = stack[:len(stack)-1]
        }
        
        // If stack is empty, no smaller element exists to the left
        if len(stack) == 0 {
            result[i] = -1
        } else {
            result[i] = stack[len(stack)-1]
        }
        
        // Push current element to stack for future comparisons
        stack = append(stack, num)
    }
    
    return result
}

func main() {
    arr := []int{4, 5, 2, 10, 8}
    fmt.Println(nearestSmallestToLeft(arr))  // Output: [-1 4 -1 2 2]
}

```

