Merge Sort is a **divide-and-conquer** algorithm that divides the array into smaller subarrays, sorts them, and then merges them back together in sorted order.

### Steps:

1. **Divide**: Recursively split the array into two halves until each subarray contains a single element.
2. **Conquer**: Sort the subarrays during the merging step.
3. **Merge**: Combine the sorted subarrays to form the sorted array.

### Time Complexity:

- **Best Case**: $O(n \log n)$
- **Worst Case**: $O(n \log n)$
- **Average Case**: $O(n \log n)$

### Space Complexity:

- Requires additional space for merging, so the space complexity is $O(n)$.

---

### Pseudocode

```plaintext
MergeSort(array, left, right)
    if left < right then
        mid = (left + right) / 2
        MergeSort(array, left, mid)
        MergeSort(array, mid + 1, right)
        Merge(array, left, mid, right)
    end if
end MergeSort

Merge(array, left, mid, right)
    n1 = mid - left + 1
    n2 = right - mid

    create leftArray[0..n1 - 1] and rightArray[0..n2 - 1]
    copy data from array[left..mid] into leftArray
    copy data from array[mid + 1..right] into rightArray

    i = 0, j = 0, k = left
    while i < n1 and j < n2 do
        if leftArray[i] <= rightArray[j] then
            array[k] = leftArray[i]
            i = i + 1
        else
            array[k] = rightArray[j]
            j = j + 1
        end if
        k = k + 1
    end while

    while i < n1 do
        array[k] = leftArray[i]
        i = i + 1
        k = k + 1
    end while

    while j < n2 do
        array[k] = rightArray[j]
        j = j + 1
        k = k + 1
    end while
end Merge
```

---

### Explanation of Pseudocode:

1. **Divide Phase (`MergeSort`)**:
    
    - Recursively divides the array into halves until each subarray has one element.
    - Uses the midpoint to split the array.
2. **Merge Phase (`Merge`)**:
    
    - Merges two sorted subarrays into a single sorted array.
    - Uses temporary arrays (`leftArray` and `rightArray`) to hold the data during the merge process.
    - Compares elements from both subarrays and places the smaller one into the original array.
    - Copies any remaining elements from the temporary arrays back into the original array.