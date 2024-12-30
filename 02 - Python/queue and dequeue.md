### Using `queue.Queue`

The `queue.Queue` is part of the `queue` module and is thread-safe. It operates on a **FIFO (First In, First Out)** basis.

#### 1. **Import and Initialize**

```python
from queue import Queue

# Create a queue
q = Queue()
```

#### 2. **Insert (Enqueue)**

```python
# Add elements to the queue
q.put(1)
q.put(2)
q.put(3)

print("Queue size:", q.qsize())  # Output: Queue size: 3
```

#### 3. **Access and Remove (Dequeue)**

```python
# Access and remove elements from the queue
print(q.get())  # Output: 1 (removes the first element)
print(q.get())  # Output: 2
```

#### 4. **Check if the Queue is Empty**

```python
# Check if the queue is empty
print(q.empty())  # Output: False (since one element is still in the queue)

# Remove the last element
q.get()

# Now the queue is empty
print(q.empty())  # Output: True
```

#### 5. **Check Size**

```python
# Add elements again
q.put(4)
q.put(5)

print("Queue size:", q.qsize())  # Output: Queue size: 2
```

---

### Using `collections.deque`

The `deque` (double-ended queue) is a flexible and efficient data structure from the `collections` module. It supports operations on both ends.

#### 1. **Import and Initialize**

```python
from collections import deque

# Create a deque
dq = deque()
```

#### 2. **Insert**

```python
# Add elements to the right (default behavior)
dq.append(1)
dq.append(2)

# Add elements to the left
dq.appendleft(0)

print(dq)  # Output: deque([0, 1, 2])
```

#### 3. **Access and Remove**

```python
# Remove elements from the right
print(dq.pop())  # Output: 2 (removes from the right)

# Remove elements from the left
print(dq.popleft())  # Output: 0 (removes from the left)

print(dq)  # Output: deque([1])
```

#### 4. **Check if an Element Exists**

```python
# Check if an element exists in the deque
print(1 in dq)  # Output: True
print(3 in dq)  # Output: False
```

#### 5. **Insert Elements at Specific Positions**

```python
# Insert element at index 1
dq.insert(1, 10)  # Adds 10 at index 1
print(dq)  # Output: deque([1, 10])
```

#### 6. **Check Size**

```python
# Check size of deque
print(len(dq))  # Output: 2
```

#### 7. **Delete**

```python
# Clear the deque
dq.clear()
print(dq)  # Output: deque([])
```

#### 8. **Reverse**

```python
# Add elements back and reverse the deque
dq.extend([1, 2, 3])
dq.reverse()
print(dq)  # Output: deque([3, 2, 1])
```

#### 9. **Rotate**

```python
# Rotate the deque
dq.rotate(1)  # Moves the last element to the front
print(dq)  # Output: deque([1, 3, 2])

dq.rotate(-2)  # Moves the first two elements to the end
print(dq)  # Output: deque([2, 1, 3])
```

---

### Summary: Key Differences Between `queue.Queue` and `deque`

|**Operation**|**`queue.Queue`**|**`deque`**|
|---|---|---|
|**Thread-safe**|Yes (suitable for multi-threaded applications)|No (use `deque` in single-threaded applications)|
|**FIFO**|Default FIFO|Can act as FIFO or LIFO|
|**Insert**|Use `put()`|Use `append()` or `appendleft()`|
|**Remove**|Use `get()`|Use `pop()` or `popleft()`|
|**Check if Empty**|Use `empty()`|Use `len()` and compare to 0|
|**Reversing and Rotation**|Not available|Supports `reverse()` and `rotate()`|

Would you like a more advanced example, such as using these in a multi-threaded scenario?