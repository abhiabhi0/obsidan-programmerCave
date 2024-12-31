- **Definition**:    
    - Generators are functions that produce a sequence of values over time instead of computing and storing them all at once.
    - Defined using the `yield` keyword instead of `return`.
    
- **How Generators Work**:
    
    - When a generator function is called, it returns a **generator object**.
    - The generator object can be iterated over to retrieve values one at a time.
- **Example**:
    
    ```python
    def countdown(n):
        while n > 0:
            yield n
            n -= 1
    
    # Using the generator
    for num in countdown(5):
        print(num)
    ```
    
    - Output: `5, 4, 3, 2, 1`
- **Key Advantages**:
    
    - **Memory Efficiency**:
        - Generators do not store all values in memory.
        - Suitable for large datasets or infinite sequences.
    - **On-the-Fly Computation**:
        - Values are generated as needed, improving performance.
    - Conserves system resources by producing items lazily.
- **Common Use Cases**:
    
    - Processing large files or datasets.
    - Generating infinite sequences (e.g., Fibonacci numbers).
    - Streaming data pipelines.
- **Generator Features**:
    
    - Can be paused and resumed, preserving state between iterations.
    - Automatically raises a `StopIteration` exception when all values are yielded.
- **Integration with Python Features**:
    
    - Work seamlessly with `for` loops for iteration.
    - Often used with tools like `map()` and `filter()` for functional programming.
    - Can be expressed using **generator expressions**:
        
        ```python
        gen_exp = (x**2 for x in range(10))
        print(list(gen_exp))  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
        ```
        
- **Key Points for Interviews**:
    
    - Be prepared to explain the difference between generators and regular functions.
    - Highlight the memory benefits of using generators for large datasets.
    - Write examples to demonstrate `yield`, generator objects, and their integration with Python features.