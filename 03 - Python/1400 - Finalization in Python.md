- **Definition**:
  - Finalization refers to the process of cleaning up resources before an object is destroyed.
  - In Python, this is typically achieved using the `__del__` method, which is called when an object is about to be garbage collected.

- **Purpose of Finalization**:
  - To perform cleanup activities such as releasing resources (e.g., closing files, network connections) associated with an object before it is destroyed.
  - Ensures that any necessary cleanup code is executed, preventing resource leaks.

### The `__del__` Method

- **Functionality**:
  - The `__del__` method is defined within a class and is automatically invoked when an object of that class is about to be garbage collected.
  - It can be overridden to implement custom cleanup logic.

- **Example of Using `__del__`**:
  ```python
  class Resource:
      def __init__(self):
          print("Resource acquired")

      def __del__(self):
          print("Resource released")

  obj = Resource()  # Output: Resource acquired
  del obj           # Output: Resource released
  ```

- **Important Considerations**:
  - **Non-deterministic Call**: The exact timing of when `__del__` is called is non-deterministic. It depends on the garbage collector and may not be called if there are circular references.
  - **Circular References**: If an object with a `__del__` method is part of a reference cycle, it may never be destroyed, leading to memory leaks.
  
### Alternatives to Finalization

- **Context Managers**:
  - The use of context managers (via the `with` statement) is recommended for managing resources. This approach ensures that resources are properly released without relying on finalizers.
  - Example:
    ```python
    class ManagedResource:
        def __enter__(self):
            print("Resource acquired")
            return self

        def __exit__(self, exc_type, exc_value, traceback):
            print("Resource released")

    with ManagedResource():
        pass  # Resource will be released automatically
    ```

### Summary of Key Points

- **Finalization** in Python primarily involves the `__del__` method, which allows for resource cleanup just before an object is destroyed.
- The `__del__` method can be used to implement custom cleanup logic but has limitations due to its non-deterministic nature and issues with circular references.
- Using context managers is often a better practice for managing resources in Python, ensuring that cleanup occurs reliably and predictably.

These notes provide a comprehensive understanding of finalization in Python and its implementation through the `__del__` method, preparing you well for discussing this topic in your interview.

Citations:
[1] https://www.geeksforgeeks.org/finalize-method-in-java-and-how-to-override-it/
[2] https://en.wikipedia.org/wiki/Finalizer
[3] https://www.scholarhat.com/tutorial/net/difference-between-finalize-and-dispose-method
[4] https://peps.python.org/pep-0442/
[5] https://pythondev.readthedocs.io/finalization.html