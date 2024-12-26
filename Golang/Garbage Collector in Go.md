[[Memory Management in Go]]
#### Overview of Garbage Collection in Go

- Go’s garbage collector (GC) uses a **concurrent, tri-color, mark-sweep** approach.
- Designed for minimal disruption to application performance while ensuring efficient memory management.

#### Key Features of Go’s Garbage Collector

1. **Concurrent**:
    
    - **Definition**: The GC operates alongside the application, avoiding a full "stop-the-world" phase.
    - **Advantages**:
        - Reduces noticeable pauses.
        - Improves performance for real-time and high-throughput systems.
    - **Implementation**:
        - Most garbage collection work is done in the background, interleaved with application execution.
        - Shorter stop-the-world pauses enhance application latency.
2. **Tri-color Marking Algorithm**:
    
    - **Color States**:
        - **White**: Objects not yet processed; candidates for garbage collection.
        - **Grey**: Objects discovered as reachable but whose descendants are not yet processed.
        - **Black**: Fully processed objects along with their descendants, confirmed as reachable.
    - **Process**:
        1. All objects start as white.
        2. Roots (global variables and local variables in active execution) are marked grey.
        3. The GC processes each grey object:
            - Scans for references to other objects.
            - Marks referenced white objects as grey.
            - Marks itself black after processing.
        4. At the end of the process:
            - All reachable objects are black.
            - All white objects are unreachable and candidates for memory reclamation.
    - **Advantages**:
        - Segregates objects by reachability.
        - Efficiently identifies and reclaims memory.
3. **Mark-Sweep Phases**:
    
    - **Mark Phase**:
        - Traverses the object graph starting from roots.
        - Uses the tri-color algorithm to identify reachable objects.
        - Operates concurrently with program execution.
    - **Sweep Phase**:
        - Reclaims memory occupied by white (unreachable) objects.
        - Runs concurrently, cleaning up small amounts of memory at a time.
    - **Benefits**:
        - Balances performance with memory usage.
        - Eliminates large memory clean-up pauses.

#### Tri-color Mark and Sweep Algorithm

- **Marking**:
    - Tags all reachable objects by traversing the object graph from the roots.
    - Ensures all active objects are marked.
- **Sweeping**:
    - Iterates through all objects.
    - Reclaims memory occupied by unmarked (white) objects.
- **Visualization**:
    - **White**: Objects not processed and considered garbage.
    - **Grey**: Objects identified but not fully processed.
    - **Black**: Fully processed objects and their descendants.

#### Write Barriers

- **Definition**: A mechanism to maintain tri-color invariants during concurrent execution.
- **Functionality**:
    - Ensures that when a black object references a white object, the white object is marked as grey.
    - Prevents prematurely collecting white objects that are still in use.
- **Importance**:
    - Essential for safe concurrent execution.
    - Avoids race conditions where objects might be reclaimed while still in use.

#### Summary of Advantages

- **Low Latency**: Concurrent operation minimizes pauses.
- **Efficient Memory Reclamation**: Tri-color marking ensures accurate identification of unused objects.
- **Safe Concurrency**: Write barriers maintain correctness during program execution.
- **Scalability**: Suitable for real-time and high-throughput applications due to its non-disruptive design.

[[Handle Memory leak in Go]]