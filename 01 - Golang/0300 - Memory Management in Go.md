#### How Go Manages Memory
- **Two-Pronged Approach**:
    - **Stack**: Local variables and function calls.
    - **Heap**: Dynamic memory allocation.
#### The Heap and the Stack
- **Stack**:
    - **Structure**: LIFO (Last In, First Out).
    - **Purpose**: <mark class="hltr-g">Stores local variables and function calls.</mark>
    - **Operation**:
        - New stack frame allocated during function calls.
        - Frame deallocated when the function finishes.
    - **Characteristics**:
        - Fast access.
        - Automatic memory management.
        - Limited size and local scope.
- **Heap**:
    - **Structure**: Unordered region of memory.
    - **Purpose**: <mark class="hltr-g">Stores data that outlives function scope.</mark>
    - **Operation**:
        - Flexible allocation and deallocation.
        - <mark class="hltr-o">Requires manual memory management.</mark>
    - **Characteristics**:
        - Slower access.
        - Used for dynamic memory allocation.

#### Memory Allocation in Go
- **Heap Management**:
    - Managed by the Garbage Collector (GC).
    - Heap size increases when needed.
    - GC identifies and discards inaccessible objects.

- **Stack Management**:    
    - Go uses dynamically sized contiguous stacks.
    - Initial stack size for goroutines: ~2KB.
    - Stack grows or shrinks dynamically as required.
    - Larger stacks are allocated with data copied over during growth.
[[0301 - Garbage Collector in Go]]
#### Benefits of Go's Memory Management
- Efficient allocation on both heap and stack.
- Dynamic stack sizing improves performance and resource utilization.
- Simplifies building high-performance applications.
