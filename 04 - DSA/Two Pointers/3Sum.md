The **3Sum problem** involves finding all unique triplets in an array that sum up to zero. This problem can be efficiently solved using the **two-pointer approach** after sorting the array.

---

### Key Observations:

1. **Sorting the Array:**
    
    - Sorting the array simplifies the problem by enabling the use of the two-pointer technique.
    - It also allows us to easily skip duplicate elements.
2. **Breaking Down the Problem:**
    
    - Fix one number, A[i]A[i], as the first element of the triplet.
    - Use two pointers to find two other numbers, A[j]A[j] and A[k]A[k], such that: A[i]+A[j]+A[k]=0A[i] + A[j] + A[k] = 0
3. **Two Pointers:**
    
    - Initialize j=i+1j = i + 1 (left pointer) and k=N−1k = N - 1 (right pointer).
    - Move the pointers based on the current sum:
        - If the sum >0> 0: Decrease kk (right pointer moves left).
        - If the sum <0< 0: Increase jj (left pointer moves right).
        - If the sum =0= 0: Add the triplet to the result and adjust both pointers to skip duplicates.
4. **Avoiding Duplicates:**
    
    - Skip duplicate values for A[i]A[i], A[j]A[j], and A[k]A[k] to ensure unique triplets.

---

### Algorithm:

1. **Input Validation:**
    
    - If the array has fewer than 3 elements, return an empty list.
2. **Sort the Array:**
    
    - Sort the array in ascending order.
3. **Iterate to Fix A[i]A[i]:**
    
    - Loop ii from 0 to N−3N-3 (last two elements are reserved for two pointers).
4. **Use Two Pointers for Remaining Pair:**
    
    - Initialize j=i+1j = i + 1, k=N−1k = N - 1.
    - While j<kj < k, calculate the sum and adjust pointers accordingly.
5. **Skip Duplicates:**
    
    - Ensure no duplicate triplets are added to the result.

---

### Code (in Python):

```python
def three_sum(nums):
    nums.sort()  # Sort the array
    n = len(nums)
    result = []

    for i in range(n - 2):
        # Skip duplicates for the fixed number
        if i > 0 and nums[i] == nums[i - 1]:
            continue

        # Two-pointer approach for the remaining two numbers
        j, k = i + 1, n - 1
        while j < k:
            current_sum = nums[i] + nums[j] + nums[k]

            if current_sum == 0:
                # Found a triplet
                result.append([nums[i], nums[j], nums[k]])

                # Skip duplicates for nums[j] and nums[k]
                while j < k and nums[j] == nums[j + 1]:
                    j += 1
                while j < k and nums[k] == nums[k - 1]:
                    k -= 1

                # Move pointers inward
                j += 1
                k -= 1
            elif current_sum < 0:
                # Move left pointer to increase sum
                j += 1
            else:
                # Move right pointer to decrease sum
                k -= 1

    return result


# Example
nums = [-1, 0, 1, 2, -1, -4]
print(three_sum(nums))  # Output: [[-1, -1, 2], [-1, 0, 1]]
```

---

### Example Walkthrough:

#### Input:

`nums = [-1, 0, 1, 2, -1, -4]`

#### Steps:

1. **Sort the Array:**
    
    - `nums = [-4, -1, -1, 0, 1, 2]`
2. **Iterate Over A[i]A[i]:**
    
    - i=0,A[i]=−4i = 0, A[i] = -4:
        
        - Two pointers j=1,k=5j = 1, k = 5.
        - Sum = −4+(−1)+2=−3-4 + (-1) + 2 = -3, move jj to 2.
        - Sum = −4+(−1)+1=−4-4 + (-1) + 1 = -4, move jj to 3.
        - Sum = −4+0+2=−2-4 + 0 + 2 = -2, move jj to 4.
        - Sum = −4+1+2=−1-4 + 1 + 2 = -1, move jj to 5 (no triplet found for A[i]=−4A[i] = -4).
    - i=1,A[i]=−1i = 1, A[i] = -1:
        
        - Two pointers j=2,k=5j = 2, k = 5.
        - Sum = −1+(−1)+2=0-1 + (-1) + 2 = 0, add `[-1, -1, 2]`.
        - Move jj to 3, kk to 4.
        - Sum = −1+0+1=0-1 + 0 + 1 = 0, add `[-1, 0, 1]`.
    - i=2,A[i]=−1i = 2, A[i] = -1: Skip duplicate A[i]A[i].
        
    - i=3,A[i]=0i = 3, A[i] = 0:
        
        - Two pointers j=4,k=5j = 4, k = 5.
        - Sum = 0+1+2=30 + 1 + 2 = 3, move kk to 4 (no triplet found for A[i]=0A[i] = 0).
3. **Result:**
    
    - `result = [[-1, -1, 2], [-1, 0, 1]]`

---

### Complexity:

1. **Sorting:**
    
    - O(Nlog⁡N)O(N \log N), where NN is the size of the array.
2. **Two-Pointer Approach:**
    
    - O(N2)O(N^2), as the two-pointer search is executed for each ii.

**Total Complexity:** O(N2)O(N^2).

This approach is efficient and avoids duplicates, ensuring only unique triplets are returned.