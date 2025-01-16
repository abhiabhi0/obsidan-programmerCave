**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXecuKwmFyuAHlDrss_eqM4yPNJXScMlQdunouChh1gnZLVoi2nKeXlLXY-UNxoFFElB-N1zulus5tRplG0a_S84FlNxDluS3vhGXNJ7OhG21vG8t1dh6m1VI654Zei2i4cxPxaFXw1eT3bexhEQc-OsjhMF?key=nGlj-zBP0XZbusbrWTyXUw)**

#### 1. **Key Characteristics of Python**
- **Dynamically Typed**: No need to declare variable types explicitly; they are determined at runtime.
- **Interpreted Language**: Python code is executed line-by-line, making it slower compared to compiled languages like C++ and Java.
    - Python interpreter is written in C, which adds overhead.
[[0200 - How Python Program is Run on a Machine]]
#### 2. **Everything is an Object**
- Every variable and entity in Python is treated as an object, even primitive types like integers and strings.

#### 3. **First-Class Citizens**
- Variables hold references to objects, not the actual data.
- Example:
    
    ```python
    a = 5
    b = a  # Both refer to the same object
    a = 6  # Now 'a' points to a new object
    ```

[[0300 - First-Class Citizens in Python]]

#### 4. **Special Identifiers**
- `_` is a special identifier used for:
    - Storing the value of the last expression in the REPL.
    - A "throw-away" variable in loops.

#### 5. **Mutable vs Immutable**
- **Immutable**: Integers, strings, tuples.
    - Example: `a = 5; a = 6` creates a new object for `6`.
- **Mutable**: Lists, dictionaries, sets.
    - Example: Appending an element modifies the same object.

#### 6. **Small Integer Caching**
- Python preallocates memory for integers in the range [-5, 256] for optimization.
- This behavior can vary based on the machine or interpreter.

#### 7. **Iterators and Iterables**
- **Iterators**: Objects with `__iter__()` and `__next__()`.
- **Iterables**: Objects that can return an iterator via `iter()`.
- Example:
    
    ```python
    lst = [1, 2, 3]
    itr = iter(lst)
    print(next(itr))  # 1
    ```
    

#### 8. **Lists**
- Store heterogeneous data, including functions.
- Support operations like slicing, negative indexing, reversing, and in-place modifications.
    
    ```python
    a = [1, 2, 3]
    a.reverse()
    print(a)  # [3, 2, 1]
    ```
    

#### 9. **Tuples**
- Immutable lists.
- Support slicing and unpacking.
- Single-element tuples must include a comma to distinguish them from expressions.
    
    ```python
    single_element = (100,)
    ```
    
[[0400 - Tuple vs List]]
#### 10. **Dictionaries**
- Key-value pairs, where keys must be immutable.
- Simultaneous addition and updating supported.
- Example:
    
    ```python
    my_dict = {'a': 1, 'b': 2}
    my_dict['c'] = 3  # Add
    ```
    

#### 11. **Sets**
- Unordered collections of unique elements.
- Do not allow nested sets because sets are mutable and unhashable.

#### 12. **Functional Python**
- Functions are objects and can be passed, returned, or stored in variables.

#### 13. **Lambda Functions**
- Anonymous functions using `lambda`.
    
    ```python
    square = lambda x: x**2
    print(square(4))  # 16
    ```
    

#### 14. **Higher-Order Functions**

- Take or return other functions.
- Example:
    
    ```python
    def add(x, y):
        return x + y
    def apply(func, x, y):
        return func(x, y)
    print(apply(add, 2, 3))  # 5
    ```
    

#### 15. **Decorators**
- Modify or extend behavior of functions.
    
    ```python
    def decorator(func):
        def wrapper():
            print("Before")
            func()
            print("After")
        return wrapper
    
    @decorator
    def greet():
        print("Hello!")
    
    greet()
    ```
    

#### 16. **Map, Filter, Reduce, Zip**
- **Map**: Apply a function to all items in an iterable.
    
    ```python
    numbers = [1, 2, 3]
    print(list(map(lambda x: x**2, numbers)))  # [1, 4, 9]
    ```
    
- **Filter**: Filter items based on a condition.
    
    ```python
    print(list(filter(lambda x: x > 1, numbers)))  # [2, 3]
    ```
    
- **Reduce**: Apply a rolling computation.
    
    ```python
    from functools import reduce
    print(reduce(lambda x, y: x + y, numbers))  # 6
    ```
    
- **Zip**: Combine multiple iterables.
    
    ```python
    a = [1, 2, 3]
    b = ['x', 'y', 'z']
    print(list(zip(a, b)))  # [(1, 'x'), (2, 'y'), (3, 'z')]
    ```
    