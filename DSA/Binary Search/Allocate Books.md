The **Allocate Books** problem is a classic **binary search on the answer** problem. The goal is to minimize the maximum number of pages allocated to any student while satisfying the constraints.

---

### Problem Breakdown:

1. **Input:**
    
    - `A` (array of pages in books): `[12, 34, 67, 90]`
    - `B` (number of students): `2`
2. **Output:**
    
    - Minimum of the maximum pages a student can be allocated: `113`
3. **Constraints:**
    
    - Each book is assigned to one student only.
    - Each student gets at least one book.
    - Books assigned to a student must be contiguous.
4. **Objective:**
    
    - Distribute books among `B` students such that the maximum number of pages assigned to any student is minimized.

---

### Approach:

The solution uses **binary search on the range of possible answers**:

1. **Search Space**:
    
    - Minimum value: `max(A)` (one student gets the book with the most pages, as a student must get at least one book).
    - Maximum value: `sum(A)` (one student gets all the books).
2. **Binary Search**:
    
    - Find the **midpoint** of the search space (`mid`).
    - Check if it is **possible** to allocate books such that no student gets more than `mid` pages.
        - If possible, `mid` could be a valid solution, but we try for a smaller value (reduce the search space).
        - If not possible, increase `mid` (increase the search space).
3. **Validation Function**:
    
    - Check if it is possible to allocate books such that no student gets more than `mid` pages:
        - Traverse the array and keep allocating books to students.
        - If adding a book exceeds the current limit (`mid`), assign the book to a new student.
        - If the number of students exceeds `B`, return `false`.

---

### Algorithm:

1. Start with `low = max(A)` and `high = sum(A)`.
2. Perform binary search:
    - Calculate `mid = (low + high) / 2`.
    - If it is possible to allocate books with `mid` as the maximum number of pages, update the result and set `high = mid - 1`.
    - Otherwise, set `low = mid + 1`.
3. Return the result.

---

### Code (in Python):

```python
def is_possible(A, n, B, max_pages):
    students_required = 1
    current_pages = 0
    
    for pages in A:
        if current_pages + pages > max_pages:
            # Allocate to a new student
            students_required += 1
            current_pages = pages
            
            if students_required > B:
                return False
        else:
            current_pages += pages
    
    return True

def allocate_books(A, B):
    if len(A) < B:
        return -1  # Not enough books for each student to get one
    
    low = max(A)  # A student must get at least one book, so max(A)
    high = sum(A)  # A student may get all books
    result = high
    
    while low <= high:
        mid = (low + high) // 2
        
        if is_possible(A, len(A), B, mid):
            result = mid
            high = mid - 1  # Try for a smaller maximum
        else:
            low = mid + 1  # Increase the range
    
    return result

# Example
A = [12, 34, 67, 90]
B = 2
print(allocate_books(A, B))  # Output: 113
```

---

### Explanation of the Code:

1. **`is_possible()`**:
    
    - This function checks if it is feasible to allocate books such that no student gets more than `max_pages`.
    - It tracks the number of students required and their current allocation.
2. **Binary Search (`allocate_books`)**:
    
    - Searches for the minimum value of `max_pages` using binary search.

---

### Trace of the Example:

- `A = [12, 34, 67, 90]`, `B = 2`
- `low = 90`, `high = 203`

#### Iterations:

1. **Mid = 146**:
    
    - Possible to allocate: Students get [12, 34, 67] and [90].
    - Update `result = 146`, reduce `high = 145`.
2. **Mid = 117**:
    
    - Possible to allocate: Students get [12, 34] and [67, 90].
    - Update `result = 117`, reduce `high = 116`.
3. **Mid = 103**:
    
    - Not possible: Students exceed `B`.
    - Increase `low = 104`.
4. **Mid = 113**:
    
    - Possible to allocate: Students get [12, 34, 67] and [90].
    - Update `result = 113`, reduce `high = 112`.
5. **Mid = 108**:
    
    - Not possible: Students exceed `B`.
    - Increase `low = 109`.

Final Result: `113`.

---

### Complexity:

1. **Time Complexity**:
    
    - Binary search on the range [max(A),sum(A)][max(A), sum(A)]: O(log⁡(sum(A)−max(A)))O(\log(sum(A) - max(A))).
    - Validation for each mid: O(N)O(N).
    - Total: O(N⋅log⁡(sum(A)−max(A)))O(N \cdot \log(sum(A) - max(A))).
2. **Space Complexity**:
    
    - O(1)O(1), as no additional space is used.

**Output**: `113`