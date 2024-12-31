- **Definition of Tuple**:
  - A tuple is a built-in data type in Python used to store multiple items in a single variable.
  - It is defined using parentheses `()` and can contain heterogeneous (different types) or homogeneous (same type) data.
  
- **Key Characteristics of Tuples**:
  - **Ordered**: The items have a defined order, and this order will not change.
  - **Immutable**: Once created, the elements of a tuple cannot be modified, added, or removed.
  - **Allow Duplicates**: Tuples can contain duplicate values.
  - **Indexed**: Items can be accessed using their index, starting from 0.

- **Creating a Tuple**:
  ```python
  mytuple = ("apple", "banana", "cherry")
  ```

- **Accessing Tuple Items**:
  ```python
  print(mytuple[1])  # Output: banana
  ```

- **Tuple with One Item**:
  To create a tuple with only one item, include a comma:
  ```python
  single_item_tuple = ("apple",)
  ```

- **Example of a Tuple**:
  ```python
  mixed_tuple = ("apple", 1, True)
  ```

- **Difference Between List and Tuple**:
  
| Feature                | List                             | Tuple                          |
|------------------------|----------------------------------|--------------------------------|
| **Syntax**             | Defined with square brackets `[]`| Defined with parentheses `()`   |
| **Mutability**         | Mutable (can change contents)   | Immutable (cannot change contents) |
| **Methods Available**  | More methods (e.g., append, remove)| Fewer methods (e.g., count, index) |
| **Performance**        | Slower due to mutability         | Faster due to immutability     |
| **Use Case**           | Use when you need a collection that can change | Use when you need a fixed collection |

- **Example of List and Tuple**:
```python
# List
my_list = [1, 2, 3]
my_list.append(4)      # Modifying the list

# Tuple
my_tuple = (1, 2, 3)
# my_tuple.append(4)   # This will raise an AttributeError
```

These concise notes should help you prepare for your interview regarding tuples and their differences from lists in Python.

Citations:
[1] https://www.w3schools.com/python/python_tuples.asp
[2] https://www.geeksforgeeks.org/python-tuples/
[3] https://www.javatpoint.com/python-tuples
[4] https://realpython.com/python-tuple/
[5] https://www.simplilearn.com/difference-between-list-and-tuple-in-python-article
[6] https://www.codechef.com/blogs/tuples-in-python
[7] https://www.guvi.in/hub/python/tuples-in-python/