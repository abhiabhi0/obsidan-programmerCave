### Sets in Python

- **Definition**:
  - A set is a mutable collection of unique elements.
  - Defined using curly braces `{}` or the `set()` constructor.

- **Characteristics**:
  - **Mutable**: You can add, remove, or modify elements after creation.
  - **Unordered**: The elements do not have a defined order.
  - **Unique Elements**: No duplicate values are allowed.

- **Common Methods**:
  - `add(element)`: Adds an element to the set.
  - `remove(element)`: Removes an element from the set; raises an error if not found.
  - `discard(element)`: Removes an element without raising an error if not found.
  - `clear()`: Removes all elements from the set.
  - Set operations: union, intersection, difference, etc.

- **Example of Set**:
  ```python
  my_set = {1, 2, 3}
  my_set.add(4)               # Adding an element
  my_set.remove(2)            # Removing an element
  print(my_set)               # Output: {1, 3, 4}
  ```

### Frozensets in Python

- **Definition**:
  - A frozenset is an immutable collection of unique elements.
  - Created using the `frozenset()` constructor.

- **Characteristics**:
  - **Immutable**: Once created, you cannot add, remove, or modify elements.
  - **Unordered**: Like sets, frozensets do not maintain any specific order.
  - **Unique Elements**: No duplicate values are allowed.
  - **Hashable**: Can be used as keys in dictionaries or as elements of other sets.

- **Common Methods**:
  - Similar to sets but without methods that modify the frozenset (e.g., no `add()` or `remove()`).
  - Supports operations like union, intersection, and difference.

- **Example of Frozenset**:
  ```python
  my_frozenset = frozenset([1, 2, 3])
  print(my_frozenset)         # Output: frozenset({1, 2, 3})
  
  # Attempting to modify will raise an error
  # my_frozenset.add(4)       # Raises AttributeError
  ```

### Key Differences Between Set and Frozenset

| Feature                | Set                             | Frozenset                       |
|------------------------|---------------------------------|----------------------------------|
| **Mutability**         | Mutable (can be modified)      | Immutable (cannot be modified)   |
| **Creation Syntax**    | `{}` or `set()`                | `frozenset()`                    |
| **Methods Available**  | Supports methods like `add()`, `remove()`, etc. | No modifying methods; only non-modifying methods |
| **Use Cases**          | Used for dynamic collections     | Used for fixed collections and as dictionary keys |
| **Hashability**        | Not hashable                    | Hashable                          |
| **Performance**        | Slightly slower due to mutability overhead | Faster for membership tests due to immutability |

### Practical Use Cases

- **Set Use Cases**:
  - Storing a collection of items where duplicates should be avoided (e.g., user IDs).
  - Performing mathematical set operations like union and intersection.

- **Frozenset Use Cases**:
  - Using as keys in dictionaries for immutable collections.
  - Storing constants that should not change throughout the program.

These notes provide a comprehensive overview of sets and frozensets in Python, highlighting their characteristics, differences, and practical examples. This should help you prepare effectively for your interview.

Citations:
[1] https://www.justacademy.co/blog-detail/difference-between-set-and-frozenset-in-python
[2] https://mathnai.com/set-vs-frozenset-in-python/
[3] https://www.programiz.com/python-programming/methods/built-in/frozenset
[4] https://www.python-engineer.com/posts/set-vs-frozentset/