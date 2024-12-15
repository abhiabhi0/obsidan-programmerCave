This problem is about finding the maximum length KK such that no subarray of length K in the array has a sum greater than or equal to BB. The solution involves using **binary search on K**, combined with a sliding window technique to efficiently calculate sums for subarrays of size K.

---

### Approach:

1. **Binary Search on KK:**
    
    - KK ranges from 11 (minimum possible length) to NN (length of the array).
    - For each KK (midpoint in the binary search), check if it is valid:
        - A subarray of size KK should have all its sums less than BB.
    - If valid, save KK as a potential answer and try for a larger KK (increase the search space).
    - If invalid, reduce the search space (decrease KK).
2. **Validation Function (`is_valid`):**
    
    - Use a **sliding window** approach to check if any subarray of size KK has a sum greater than or equal to BB.
    - This is done in O(N)O(N) time for each KK.
3. **Optimization:**
    
    - Instead of iterating over all possible KK values (from 11 to NN), binary search reduces the number of checks to O(log⁡N)O(\log N), and the sliding window ensures each check runs in O(N)O(N).
    - Total complexity: O(Nlog⁡N)O(N \log N).

---

### Algorithm:

1. Initialize:
    
    - l=1l = 1 (minimum length of KK).
    - r=Nr = N (maximum length of KK).
    - ans=0ans = 0 (to store the maximum valid KK).
2. Perform Binary Search:
    
    - Compute mid=(l+r)//2mid = (l + r) // 2.
    - Use `is_valid` to check if any subarray of size midmid has a sum ≥B\geq B.
    - If `is_valid` returns `True`:
        - This midmid is invalid, so decrease the range (r=mid−1r = mid - 1).
    - If `is_valid` returns `False`:
        - This midmid is valid, save midmid as a potential answer and try for a larger KK (l=mid+1l = mid + 1).
3. Return ansans.
    

---

### Code (in Python):

```python
def is_valid(arr, K, B):
    # Check if there is any subarray of size K with sum >= B
    window_sum = sum(arr[:K])  # Sum of the first window of size K
    if window_sum >= B:
        return True

    for i in range(K, len(arr)):
        window_sum += arr[i] - arr[i - K]  # Slide the window
        if window_sum >= B:
            return True

    return False


def max_length_no_subarray_with_sum_gte_B(arr, B):
    n = len(arr)
    l, r = 1, n  # Binary search range
    ans = 0

    while l <= r:
        mid = (l + r) // 2
        if is_valid(arr, mid, B):
            # If mid is invalid, try smaller lengths
            r = mid - 1
        else:
            # If mid is valid, save it and try larger lengths
            ans = mid
            l = mid + 1

    return ans


# Example
arr = [1, 2, 3, 4, 5]
B = 11
print(max_length_no_subarray_with_sum_gte_B(arr, B))  # Output: 2
```

---

### Example Walkthrough:

#### Input:

- `arr = [1, 2, 3, 4, 5]`
- `B = 11`

#### Binary Search:

1. **Initial Range**:
    
    - l=1,r=5l = 1, r = 5 (array length), ans=0ans = 0.
2. **Mid = 3**:
    
    - Check if subarrays of size 3 have a sum ≥11\geq 11.
    - Subarrays:
        - [1, 2, 3] → Sum = 6 (valid)
        - [2, 3, 4] → Sum = 9 (valid)
        - [3, 4, 5] → Sum = 12 (**invalid**).
    - Result: Invalid (is_valid=Trueis\_valid = True).
    - Update r=3−1=2r = 3 - 1 = 2.
3. **Mid = 1**:
    
    - Check if subarrays of size 1 have a sum ≥11\geq 11.
    - Subarrays:
        - [1], [2], [3], [4], [5] → All valid.
    - Result: Valid (is_valid=Falseis\_valid = False).
    - Update ans=1,l=1+1=2ans = 1, l = 1 + 1 = 2.
4. **Mid = 2**:
    
    - Check if subarrays of size 2 have a sum ≥11\geq 11.
    - Subarrays:
        - [1, 2] → Sum = 3 (valid)
        - [2, 3] → Sum = 5 (valid)
        - [3, 4] → Sum = 7 (valid)
        - [4, 5] → Sum = 9 (valid).
    - Result: Valid (is_valid=Falseis\_valid = False).
    - Update ans=2,l=2+1=3ans = 2, l = 2 + 1 = 3.

#### Final Answer:

- ans=2ans = 2.

---

### Complexity:

1. **Binary Search**:
    
    - O(log⁡N)O(\log N) iterations.
2. **Sliding Window**:
    
    - Each iteration checks subarrays in O(N)O(N).

**Total**: O(Nlog⁡N)O(N \log N).

---

### Example Output:

```plaintext
2
```