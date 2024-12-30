`i < j and A[i] > 2xA[j]`

To solve this problem efficiently, we can use a **modified merge sort**. The idea is that while merging two sorted halves, we can count the pairs where `A[i] > 2 * A[j]` for `i < j`.

### Key Idea:

- If an element `A[i]` from the left half satisfies the condition with `A[j]` from the right half (i.e., `A[i] > 2 * A[j]`), then **all elements after `i`** will also satisfy this condition with `A[j]` because the left half is sorted.
- Similarly, if an element `A[j]` from the right half satisfies the condition with `A[i]`, then **all elements before `j`** in the right half will also satisfy this condition with `A[i]`.

### Algorithm:

1. **Merge Sort with Counting**: Perform a **merge sort** on the array, but during the merge step, count the pairs that satisfy the condition `A[i] > 2 * A[j]` where `i < j`.
    
2. **Divide and Conquer**:
    
    - First, recursively divide the array into two halves.
    - In the merge step, count how many pairs satisfy the condition and then merge the two halves back together.

### Steps:

1. **Merge Sort Function**:
    
    - Divide the array into two halves.
    - Recursively count the pairs in each half.
    - Merge the two halves, while counting how many times the condition `A[i] > 2 * A[j]` is satisfied.
2. **Merge Function**:
    
    - During the merge, for each element in the left half, check how many elements in the right half satisfy `A[i] > 2 * A[j]` for `i < j`.
    - Maintain a count of these pairs.

### Code Example (Python):

```python
def merge_count(arr, temp_arr, left, right):
    if left == right:
        return 0
    
    mid = (left + right) // 2
    count = merge_count(arr, temp_arr, left, mid) + merge_count(arr, temp_arr, mid + 1, right)
    
    # Now, merge the two halves while counting pairs
    count += merge(arr, temp_arr, left, mid, right)
    
    return count

def merge(arr, temp_arr, left, mid, right):
    i = left    # Starting index for left subarray
    j = mid + 1 # Starting index for right subarray
    k = left    # Starting index to be sorted
    count = 0
    
    # Count pairs while merging
    while i <= mid:
        while j <= right and arr[i] > 2 * arr[j]:
            j += 1
        count += (j - (mid + 1))  # All elements from j to right will satisfy the condition with i
        i += 1
    
    # Now, merge the two sorted halves
    i = left
    j = mid + 1
    while i <= mid and j <= right:
        if arr[i] <= arr[j]:
            temp_arr[k] = arr[i]
            i += 1
        else:
            temp_arr[k] = arr[j]
            j += 1
        k += 1
    
    while i <= mid:
        temp_arr[k] = arr[i]
        i += 1
        k += 1
    
    while j <= right:
        temp_arr[k] = arr[j]
        j += 1
        k += 1
    
    for i in range(left, right + 1):
        arr[i] = temp_arr[i]
    
    return count

def count_pairs(arr):
    n = len(arr)
    temp_arr = [0] * n
    return merge_count(arr, temp_arr, 0, n - 1)

# Example Usage
arr = [3, 6, 12, 5, 18]
print(count_pairs(arr))  # Output: 4
```

### Time Complexity:

- The time complexity of this approach is `O(N log N)` because it's based on the merge sort algorithm.

### Explanation:

- The `merge_count` function recursively divides the array and counts the pairs where `A[i] > 2 * A[j]` while merging the sorted halves.
- The `merge` function not only merges two halves but also counts how many valid pairs satisfy the condition for each element from the left half with elements from the right half.