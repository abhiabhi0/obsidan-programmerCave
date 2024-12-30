To solve the **Kth Missing Positive Number** problem, we use the property that the count of missing numbers less than or equal to a number in a sorted array can be derived mathematically. This allows us to efficiently find the KK-th missing number using binary search.

---

### Key Observations:

1. **Missing Numbers Less Than a Given Element**:
    
    - For an element at index ii in the array `Arr`, the count of missing numbers less than Arr[i]Arr[i] is: missing_count=Arr[i]−(i+1)\text{missing\_count} = Arr[i] - (i + 1)
    - This is because the expected value of the ii-th element in a perfectly continuous sequence is i+1i + 1, and the difference Arr[i]−(i+1)Arr[i] - (i + 1) gives the number of missing elements up to Arr[i]Arr[i].
2. **Using Binary Search**:
    
    - We perform binary search to find the **largest index** midmid such that: Arr[mid]−(mid+1)<KArr[mid] - (mid + 1) < K
    - This index midmid represents the position in the array where the number of missing elements up to that index is still less than KK.
3. **Finding the KK-th Missing Number**:
    
    - If the largest index midmid is found, the total missing numbers up to `Arr[mid]` is missing_count=Arr[mid]−(mid+1)\text{missing\_count} = Arr[mid] - (mid + 1).
    - The remaining numbers needed to reach KK are: remaining=K−missing_count\text{remaining} = K - \text{missing\_count}
    - The KK-th missing number is: result=Arr[mid]+remaining\text{result} = Arr[mid] + \text{remaining}

---

### Steps:

1. **Input Handling**:
    
    - Let `Arr` be the sorted array and KK the desired missing number index.
2. **Binary Search**:
    
    - Use binary search to find the largest index midmid where Arr[mid]−(mid+1)<KArr[mid] - (mid + 1) < K.
3. **Compute Result**:
    
    - After binary search, calculate the remaining missing numbers and return the result.

---

### Code (in Python):

```python
def find_kth_missing(arr, k):
    n = len(arr)
    left, right = 0, n - 1
    result = -1

    # Binary search to find the largest index with missing_count < k
    while left <= right:
        mid = (left + right) // 2
        missing_count = arr[mid] - (mid + 1)

        if missing_count < k:
            result = mid  # Update potential answer
            left = mid + 1
        else:
            right = mid - 1

    # Calculate the remaining missing numbers after arr[result]
    if result == -1:
        return k  # All missing numbers are less than arr[0]
    else:
        missing_count = arr[result] - (result + 1)
        remaining = k - missing_count
        return arr[result] + remaining


# Example
arr = [2, 3, 4, 7, 11]
k = 5
print(find_kth_missing(arr, k))  # Output: 9
```

---

### Example Walkthrough:

#### Input:

- `Arr = [2, 3, 4, 7, 11]`
- K=5K = 5

---

#### Step 1: Calculate Missing Numbers:

- For each index ii:
    - Arr[0]−(0+1)=2−1=1Arr[0] - (0 + 1) = 2 - 1 = 1 (1 missing number).
    - Arr[1]−(1+1)=3−2=1Arr[1] - (1 + 1) = 3 - 2 = 1 (still 1 missing number).
    - Arr[2]−(2+1)=4−3=1Arr[2] - (2 + 1) = 4 - 3 = 1 (still 1 missing number).
    - Arr[3]−(3+1)=7−4=3Arr[3] - (3 + 1) = 7 - 4 = 3 (3 missing numbers).
    - Arr[4]−(4+1)=11−5=6Arr[4] - (4 + 1) = 11 - 5 = 6 (6 missing numbers).

#### Step 2: Binary Search:

- Search range: left=0left = 0, right=4right = 4.
    
- Midpoint mid=2mid = 2:
    
    - missing_count=Arr[2]−(2+1)=4−3=1missing\_count = Arr[2] - (2 + 1) = 4 - 3 = 1.
    - missing_count<K(5)missing\_count < K (5), so update result=2result = 2, and move left=3left = 3.
- Midpoint mid=3mid = 3:
    
    - missing_count=Arr[3]−(3+1)=7−4=3missing\_count = Arr[3] - (3 + 1) = 7 - 4 = 3.
    - missing_count<K(5)missing\_count < K (5), so update result=3result = 3, and move left=4left = 4.
- Midpoint mid=4mid = 4:
    
    - missing_count=Arr[4]−(4+1)=11−5=6missing\_count = Arr[4] - (4 + 1) = 11 - 5 = 6.
    - missing_count>K(5)missing\_count > K (5), so move right=3right = 3.

#### Step 3: Compute Result:

- Largest index result=3result = 3.
- Missing numbers up to `Arr[3]` = Arr[3]−(3+1)=3Arr[3] - (3 + 1) = 3.
- Remaining numbers = K−3=2K - 3 = 2.
- Answer = Arr[3]+2=7+2=9Arr[3] + 2 = 7 + 2 = 9.

---

### Output:

```plaintext
9
```

---

### Complexity:

1. **Binary Search:**
    
    - O(log⁡N)O(\log N), where NN is the size of the array.
2. **Overall Complexity:**
    
    - O(log⁡N)O(\log N).

This solution efficiently handles large arrays due to the logarithmic complexity of binary search.