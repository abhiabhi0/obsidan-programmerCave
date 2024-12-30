Bubble Sort is a simple sorting algorithm that repeatedly compares adjacent elements and swaps them if they are in the wrong order. This process continues until the list is sorted. The algorithm gets its name because smaller elements "bubble" to the top of the list (beginning) with each pass.

### Steps:

1. Start from the first element.
2. Compare each pair of adjacent elements.
3. Swap the elements if they are in the wrong order.
4. Repeat the process for all elements until no swaps are needed.

### Time Complexity:

- **Best Case:** $O(n)$ (when the array is already sorted)
- **Worst Case:** $O(n^2)$
- **Average Case:** $O(n^2)$

### Pseudocode:

```plaintext
BubbleSort(array)
    n = length(array)
    for i from 0 to n - 1 do
        swapped = false
        for j from 0 to n - i - 2 do
            if array[j] > array[j + 1] then
                swap(array[j], array[j + 1])
                swapped = true
        end for
        if not swapped then
            break
    end for
end BubbleSort
```

### Explanation of Pseudocode:

1. **Outer Loop (`i`)**: Runs through the array for $n$ iterations.
2. **Inner Loop (`j`)**: Compares and swaps adjacent elements if needed.
3. **Optimization with `swapped`**: If no elements are swapped in an iteration, the array is already sorted, and the algorithm exits early.

This makes Bubble Sort inefficient for large datasets but straightforward for small or nearly sorted data.