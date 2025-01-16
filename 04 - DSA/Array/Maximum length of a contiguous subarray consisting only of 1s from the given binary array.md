`0 0 1 0 1 1 0 0 1 1 1 1 1 1 0 0 0 0 1 1 0`

```python
def max_length_subarray_of_ones(arr):
    max_length = 0
    current_length = 0
    
    for num in arr:
        if num == 1:
            current_length += 1
        else:
            max_length = max(max_length, current_length)
            current_length = 0
    
    # Final check to ensure we consider trailing ones
    max_length = max(max_length, current_length)
    
    return max_length

# Given binary array
binary_array = [0, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 1, 1, 0]
result = max_length_subarray_of_ones(binary_array)
print("Maximum length of contiguous subarray of ones:", result)
```

