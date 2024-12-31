- **Private Heap**:
  - Python uses a private heap space dedicated to the Python interpreter.
  - All Python objects and data structures are stored in this heap.
  - The operating system cannot allocate this memory to other processes.

- **Python Memory Manager**:
  - The memory manager handles memory allocation and deallocation.
  - It interacts with the operating system's memory manager to ensure enough space is available in the private heap.
  - Various components manage dynamic storage aspects like sharing, segmentation, preallocation, and caching.

### Memory Allocation Techniques

1. **Static Memory Allocation**:
   - Fixed memory size for variables declared within functions or classes.
   - Stored in stack memory; each function call creates a new frame on the call stack.

2. **Dynamic Memory Allocation**:
   - Memory is allocated at runtime as needed.
   - Utilizes a combination of techniques, including:
     - **Memory Pools**: 
       - Manages small memory blocks efficiently by reusing available blocks.
       - Reduces overhead associated with frequent allocation and deallocation.

3. **Memory Fragmentation**:
   - Occurs when memory is divided into small, non-contiguous blocks.
   - Python mitigates fragmentation through strategies like compacting memory and combining adjacent free blocks.

### Garbage Collection

- **Automatic Memory Management**:
  - Python automatically reclaims memory that is no longer in use through garbage collection.
  
- **Reference Counting**:
  - Each object has a reference count that tracks how many references point to it.
  - When the reference count drops to zero, the object is deallocated.

- **Garbage Collector**:
  - Runs periodically to identify and collect objects with a reference count of zero.
  - Uses techniques like mark-and-sweep to detect unreachable objects.

### Memory Management for Data Structures

- **Lists**:
  - Dynamic arrays that can grow or shrink as needed.
  - When resizing, Python allocates new memory and copies existing elements to the new location.

- **Dictionaries**:
  - Memory is allocated for key-value pairs; resizing occurs when adding new elements beyond current capacity.

- **Sets**:
  - Similar to dictionaries; they resize and rehash elements when exceeding initial capacity.

### Function Memory Management

- **Local Variables and Stack Frames**:
  - Each function call creates a new frame on the call stack for local variables.
  - Local variables are deallocated automatically when the function returns.

### CPython's Memory Management

- **Block, Pool, and Arena Structure**:
  - CPython divides memory into blocks (smallest unit), pools (sets of blocks), and arenas (largest chunks).
  - This structure optimizes memory allocation for small-sized data typical in Python programs.

### Summary of Key Points

- Python manages memory through a private heap and an internal memory manager, which dynamically allocates and deallocates memory using reference counting and garbage collection.
- Data structures like lists, dictionaries, and sets have specific memory management characteristics tailored for their usage patterns.
- Understanding these concepts helps optimize performance and prevent memory-related issues in Python applications.

These notes provide a comprehensive overview of how memory is managed in Python, equipping you with essential information for your interview preparation.

Citations:
[1] https://www.squash.io/how-to-manage-memory-with-python/
[2] https://docs.python.org/pt-br/3.8/c-api/memory.html
[3] https://www.scaler.com/topics/memory-management-in-python/
[4] https://www.honeybadger.io/blog/memory-management-in-python/
[5] https://docs.python.org/it/3.7/c-api/memory.html