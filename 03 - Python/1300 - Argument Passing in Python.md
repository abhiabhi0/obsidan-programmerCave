**Parameter Passing Mechanism**:
  - Python uses a mechanism known as **"pass by object reference"** or **"call by object reference"**.
  - This means that when you pass an argument to a function, you are passing a reference to the object, not the actual object itself.

### Key Concepts

1. **Pass by Value vs. Pass by Reference**:
   - **Pass by Value**: A copy of the actual value is passed to the function. Changes made to the parameter do not affect the original variable.
   - **Pass by Reference**: A reference (or address) of the actual variable is passed to the function. Changes made to the parameter affect the original variable.
   - In Python, it is neither strictly pass by value nor pass by reference; instead, it is a combination of both.

2. **Mutable vs. Immutable Objects**:
   - **Mutable Objects**: These are objects that can be changed after they are created (e.g., lists, dictionaries).
     - Changes made to mutable objects within a function will reflect outside the function.
     - Example:
       ```python
       def modify_list(lst):
           lst.append(4)

       my_list = [1, 2, 3]
       modify_list(my_list)
       print(my_list)  # Output: [1, 2, 3, 4]
       ```
   - **Immutable Objects**: These are objects that cannot be changed after they are created (e.g., integers, strings, tuples).
     - Changes made to immutable objects within a function do not affect the original object.
     - Example:
       ```python
       def modify_integer(x):
           x += 10
           print("Inside function:", x)

       num = 5
       modify_integer(num)
       print("Outside function:", num)  # Output: 5
       ```

3. **Object References**:
   - When an argument is passed to a function, what is actually passed is a reference to the object in memory.
   - If you reassign the parameter to a new object inside the function, it does not affect the original object outside.
   - Example:
     ```python
     def reassign_string(s):
         s = "New String"
         print("Inside function:", s)

     original_string = "Old String"
     reassign_string(original_string)
     print("Outside function:", original_string)  # Output: Old String
     ```

### Important Points

- **Function Parameters as Local Variables**:
  - Inside a function, parameters act as local variables. Assigning a new value to them does not change the argument used in the call.
  
- **Effects of Mutability**:
  - For mutable objects, modifications within a function will affect the original object since both refer to the same memory location.
  - For immutable objects, any operation that seems to modify them actually creates a new object.

- **Practical Implications**:
  - Understanding this mechanism helps avoid unintended side effects when passing mutable objects to functions.
  - It also clarifies why certain operations on immutable types do not change their values outside of their respective functions.

### Summary

- In Python, arguments are passed using "pass by object reference."
- Mutable objects can be modified within functions and reflect changes outside; immutable objects cannot be modified in place.
- This understanding is crucial for writing effective and bug-free Python code.

These notes should provide you with a comprehensive understanding of how arguments are passed in Python, preparing you well for your interview.

Citations:
[1] https://qissba.com/parameter-passing-method-in-python/
[2] https://www.oreilly.com/library/view/learning-python/1565924649/ch04s04.html
[3] https://www.javatpoint.com/arguments-and-parameters-in-python
[4] https://python-course.eu/python-tutorial/passing-arguments.php
[5] https://realpython.com/videos/parameter-passing/
[6] https://www.geeksforgeeks.org/pass-by-reference-vs-value-in-python/