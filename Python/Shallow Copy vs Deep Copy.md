
1. **Shallow Copy**: A shallow copy creates a new object, but instead of copying the elements of the original object, it ==creates references to the objects found in the original.== This means that ==changes made to the elements of the copied object will affect the original object, and vice versa.== Shallow copying is done using the `copy()` method or the `copy` module's `copy()` function.

```python
import copy

original_list = [[1, 2, 3], [4, 5, 6]]
shallow_copied_list = copy.copy(original_list)

original_list[0][0] = 'a'
print(original_list)         # Output: [['a', 2, 3], [4, 5, 6]]
print(shallow_copied_list)   # Output: [['a', 2, 3], [4, 5, 6]]
```

In this example, modifying `original_list` affects `shallow_copied_list`, as they share references to the same inner lists.

2. **Deep Copy**: A deep copy, on the other hand, creates a ==new object and recursively copies all objects found in the original object, creating separate copies of everything.== This means that ==changes made to the elements of the copied object will not affect the original object, and vice versa==. Deep copying is done using the `copy()` method or the `copy` module's `deepcopy()` function.

```python
import copy

original_list = [[1, 2, 3], [4, 5, 6]]
deep_copied_list = copy.deepcopy(original_list)

original_list[0][0] = 'a'
print(original_list)        # Output: [['a', 2, 3], [4, 5, 6]]
print(deep_copied_list)     # Output: [[1, 2, 3], [4, 5, 6]]
```

Here, modifying `original_list` doesn't affect `deep_copied_list`, as they are completely independent copies of each other.

In summary, shallow copy creates a new object with references to the original's nested objects, while deep copy creates a new object with copies of the original's nested objects, recursively. The choice between shallow and deep copy depends on whether you need independent copies or if you're okay with shared references.