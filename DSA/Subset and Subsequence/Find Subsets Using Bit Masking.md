## **Concept**

1. If an array has `n` elements, the total number of subsets (including the empty subset) is 2n2^n.
2. Each subset can be represented by an **n-bit binary number**:
    - Each bit in the binary number represents whether an element at that position is included in the subset.
    - **Bit set (1)** → Include the element.
    - **Bit not set (0)** → Exclude the element.
3. Loop through all numbers from `0` to 2n−12^n - 1.
4. Use a nested loop to check the position of each bit using bitwise **AND**.

---

## **Steps**

1. **Outer Loop**: Iterate through all numbers from `0` to 2n−12^n - 1 (where `n` is the size of the array).
2. **Inner Loop**: For each number:
    - Use `(i >> j) & 1` to check if the `j-th` bit is set.
    - If set, include the `j-th` element of the array in the subset.
3. Store or print the subset.

---

## **Diagram**

Given array: `[a, b, c]`  
The total number of subsets = 23=82^3 = 8.  
Binary representation and subsets:

|**i (binary)**|**Subset**|
|---|---|
|000|`{}` (empty set)|
|001|`{c}`|
|010|`{b}`|
|011|`{b, c}`|
|100|`{a}`|
|101|`{a, c}`|
|110|`{a, b}`|
|111|`{a, b, c}`|

Here:

- `001` → Include only the 3rd element (`c`).
- `101` → Include 1st and 3rd elements (`a, c`).

---

## **Python Code**

```python
def find_subsets(arr):
    n = len(arr)
    subsets = []

    # Loop from 0 to 2^n - 1
    for i in range(1 << n):  # 1 << n is 2^n
        subset = []
        for j in range(n):
            if (i >> j) & 1:  # Check if the j-th bit is set
                subset.append(arr[j])
        subsets.append(subset)

    return subsets

# Example Usage
arr = ["a", "b", "c"]
result = find_subsets(arr)
for subset in result:
    print(subset)
```

### **Output**:

```
[]
['a']
['b']
['a', 'b']
['c']
['a', 'c']
['b', 'c']
['a', 'b', 'c']
```

---

## **Go Code**

```go
package main

import "fmt"

func findSubsets(arr []string) [][]string {
    n := len(arr)
    subsets := [][]string{}

    // Loop from 0 to 2^n - 1
    for i := 0; i < (1 << n); i++ {
        subset := []string{}
        for j := 0; j < n; j++ {
            if (i>>j)&1 == 1 { // Check if the j-th bit is set
                subset = append(subset, arr[j])
            }
        }
        subsets = append(subsets, subset)
    }
    return subsets
}

func main() {
    arr := []string{"a", "b", "c"}
    result := findSubsets(arr)
    for _, subset := range result {
        fmt.Println(subset)
    }
}
```

### **Output**:

```
[]
[a]
[b]
[a b]
[c]
[a c]
[b c]
[a b c]
```

---

## **Key Points to Remember**

1. **Bit Masking**:
    - Use the binary representation of numbers to determine which elements are included in a subset.
    - `(i >> j) & 1` checks if the `j-th` bit is set in `i`.
2. **Outer Loop**:
    - Iterates from `0` to 2n−12^n - 1.
3. **Inner Loop**:
    - Iterates through each bit of `i` and checks whether to include the corresponding array element.
4. **Time Complexity**: O(n⋅2n)O(n \cdot 2^n)
    - 2n2^n subsets, and checking each element takes O(n)O(n).
5. **Space Complexity**: O(2n⋅n)O(2^n \cdot n)
    - Space to store all subsets.