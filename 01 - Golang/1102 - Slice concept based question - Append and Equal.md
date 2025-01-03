[[1100 - Slice]]
### Code Breakdown

```go
package main

import (
    "fmt"
)

func main() {
    var x []int
    x = append(x, 1)
    x = append(x, 2)
    x = append(x, 3)
    x = append(x, 4)

    y := x

    x[0] = 0
    y = append(y, 5)
    x = append(x, 6)

    fmt.Println(x) // Output: [0 2 3 4 6]
    fmt.Println(y) // Output: [1 2 3 4 5]
}
```

### Internal Memory Representation of Slices

In Go, a slice is a descriptor that consists of three components:
- **Pointer**: A pointer to the underlying array where the slice's elements are stored.
- **Length**: The number of elements currently in the slice.
- **Capacity**: The total number of elements that the slice can hold before needing to allocate more memory.

### Step-by-Step Execution

1. **Initialization**:
   - `var x []int` initializes `x` as a nil slice. At this point, it has no underlying array allocated.

2. **Appending Elements**:
   - Each call to `append` dynamically allocates memory for the underlying array if necessary. 
   - After appending `1`, `2`, `3`, and `4`, the internal representation of `x` would look like this:
     - Pointer: points to an array `[1, 2, 3, 4]`
     - Length: `4`
     - Capacity: `4`

3. **Assigning y**:
   - `y := x` creates a new slice `y` that points to the same underlying array as `x`. Both slices now share the same pointer to `[1, 2, 3, 4]`, and both have a length of `4` and a capacity of `4`.

4. **Modifying x**:
   - `x = 0` changes the first element of the underlying array to `0`. Now, both slices reflect this change because they share the same underlying array. The state is now:
     - Array: `[0, 2, 3, 4]`
     - Lengths remain unchanged for both slices.

5. **Appending to y**:
   - When executing `y = append(y, 5)`, Go checks if there is enough capacity in the underlying array.
     - Since there is no capacity left (current length is `4`), Go allocates a new array with increased capacity (typically double).
     - The new state for `y` becomes:
       - New Array: `[0, 2, 3, 4, 5]`
       - Length: `5`
       - Capacity: increased (let's assume it becomes `8`).

6. **Appending to x**:
   - Next, when executing `x = append(x, 6)`, since there is still enough capacity in the original array for `x`, it appends without needing to allocate a new array.
   - The state for `x` now becomes:
     - Array: `[0, 2, 3, 4, 6]`
     - Length: `5`
     - Capacity remains unchanged at `8`.

### Final Outputs

- After all operations:
   - **For x**: The output will be `[0, 2, 3, 4, 6]`.
   - **For y**: The output will be `[1, 2, 3, 4, 5]`.

### Summary of Memory Changes

- Initially, both slices (`x` and `y`) point to the same underlying array.
- Modifying one slice affects both until one of them is appended to (which causes a reallocation for that slice).
- After appending to either slice that exceeds its capacity, a new underlying array is allocated for that slice while the other remains unchanged.

This behavior highlights how Go manages memory for slices and demonstrates their reference semantics effectively. Understanding these details is crucial for writing efficient Go code and managing memory usage effectively.

Citations:
[1] https://oilbeater.com/en/2024/03/04/golang-slice-performance/
[2] https://dev.to/doziestar/mastering-memory-the-art-of-memory-management-and-garbage-collection-in-go-5292
[3] https://www.reddit.com/r/golang/comments/176rbi8/how_slice_with_type_of_any_is_stored_in_memory/
[4] https://www.calsoftinc.com/blogs/golang-memory-management.html
[5] https://go.dev/blog/slices-intro