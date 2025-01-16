### Using `dict`

#### 1. **Insert/Update**

```python
# Create a dictionary
my_dict = {}

# Insert key-value pairs
my_dict['a'] = 1
my_dict['b'] = 2

# Update an existing key
my_dict['a'] = 3

print(my_dict)  # Output: {'a': 3, 'b': 2}
```

---

#### 2. **Access**

```python
# Access an existing key
print(my_dict['a'])  # Output: 3

# Accessing a non-existent key (raises KeyError)
# print(my_dict['c'])  # Uncommenting this line will raise a KeyError
```

---

#### 3. **Check if a Key Exists**

```python
# Check if a key exists in the dictionary
print('a' in my_dict)  # Output: True
print('c' in my_dict)  # Output: False
```

---

#### 4. **Check if a Value Exists**

```python
# Check if a value exists in the dictionary
print(3 in my_dict.values())  # Output: True
print(5 in my_dict.values())  # Output: False
```

---

#### 5. **Delete**

```python
# Delete a key
del my_dict['b']
print(my_dict)  # Output: {'a': 3}

# Attempting to delete a non-existent key (raises KeyError)
# del my_dict['c']  # Uncommenting this line will raise a KeyError
```

---

#### 6. **Using `.get()` for Safe Access**

```python
# Use .get() to safely access a key without raising KeyError
print(my_dict.get('a', 0))  # Output: 3
print(my_dict.get('c', 0))  # Output: 0 (default value if key doesn't exist)
```

---

### Using `defaultdict`

#### 1. **Create a `defaultdict`**

```python
from collections import defaultdict

# Create a defaultdict with default value as an empty list
my_defaultdict = defaultdict(list)
```

---

#### 2. **Insert/Access**

```python
# Insert values by appending to the list
my_defaultdict['a'].append(1)
my_defaultdict['b'].append(2)

# Access a non-existent key (automatically initializes with the default value)
print(my_defaultdict['c'])  # Output: []

print(my_defaultdict)  # Output: defaultdict(<class 'list'>, {'a': [1], 'b': [2], 'c': []})
```

---

#### 3. **Update**

```python
# Append more values to existing keys
my_defaultdict['a'].append(3)

print(my_defaultdict)  # Output: defaultdict(<class 'list'>, {'a': [1, 3], 'b': [2], 'c': []})
```

---

#### 4. **Check if a Key Exists**

```python
# Check if a key exists in the defaultdict
print('a' in my_defaultdict)  # Output: True
print('d' in my_defaultdict)  # Output: False
```

---

#### 5. **Check if a Value Exists**

```python
# Check if a value exists in the defaultdict (requires iteration over all values)
print(1 in my_defaultdict.values())  # Output: False (since values are lists)
print([1, 3] in my_defaultdict.values())  # Output: True
```

---

#### 6. **Delete**

```python
# Delete a key
del my_defaultdict['b']
print(my_defaultdict)  # Output: defaultdict(<class 'list'>, {'a': [1, 3], 'c': []})
```

---

#### 7. **Default Factory Examples**

You can customize the default value type when creating the `defaultdict`:

```python
# defaultdict with default value as integer
default_int = defaultdict(int)
default_int['x'] += 1  # Initializes as 0, then increments
print(default_int)  # Output: defaultdict(<class 'int'>, {'x': 1})

# defaultdict with default value as string
default_str = defaultdict(str)
default_str['key'] += 'hello'
print(default_str)  # Output: defaultdict(<class 'str'>, {'key': 'hello'})
```

---

### Summary: Key Differences Between `dict` and `defaultdict`

|**Operation**|**`dict`**|**`defaultdict`**|
|---|---|---|
|**Missing Key Access**|Raises `KeyError`|Automatically creates the key with a default value|
|**Key Check**|Use `key in dict`|Use `key in defaultdict`|
|**Value Check**|Use `value in dict.values()`|Use `value in defaultdict.values()`|
|**Use Case**|When you explicitly control key creation and access|When you need automatic handling for missing keys|

Would you like to see more examples, such as iterating through keys and values or combining dictionaries?