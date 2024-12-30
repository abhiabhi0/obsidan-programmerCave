### Problem

Given a list of strings `strs`, find the **longest common prefix** shared by all strings.

### Approach 1: Character-by-Character Check

1. Traverse each character index `i` (starting from 0).
2. For each index `i`, check if all strings have the same character at that position:
    - If any string is shorter than `i` or has a different character, break the loop.
3. Return the prefix formed.

**Time Complexity**: O(s)O(s), where ss is the total length of all strings.  
**Space Complexity**: O(1)O(1).

---

### Approach 2: Binary Search on Index

1. Use binary search on the possible prefix length:
    - Calculate the middle index `mid` of the range `[0, minLen]`, where `minLen` is the length of the shortest string.
    - Check if all strings share a prefix of length `mid`.
2. If they do, increase the search range (`l = mid + 1`); otherwise, decrease it (`r = mid - 1`).
3. The result is the prefix of length stored from the last valid `mid`.

**Time Complexity**: O(s⋅log⁡n)O(s \cdot \log n), where nn is the number of strings.  
**Space Complexity**: O(1)O(1).

---

### Python Implementation

#### Character-by-Character Check

```python
def longest_common_prefix(strs: list[str]) -> str:
    if not strs:
        return ""
    
    prefix = ""
    for i in range(len(min(strs, key=len))):
        char = strs[0][i]
        if all(s[i] == char for s in strs):
            prefix += char
        else:
            break
    return prefix

# Usage
strs = ["flower", "flow", "flight"]
print(longest_common_prefix(strs))  # Output: "fl"
```

#### Binary Search on Index

```python
def longest_common_prefix_binary(strs: list[str]) -> str:
    if not strs:
        return ""
    
    def is_common_prefix(length: int) -> bool:
        prefix = strs[0][:length]
        return all(s.startswith(prefix) for s in strs)
    
    min_len = len(min(strs, key=len))
    left, right = 0, min_len
    while left <= right:
        mid = (left + right) // 2
        if is_common_prefix(mid):
            left = mid + 1
        else:
            right = mid - 1
    
    return strs[0][:right]

# Usage
strs = ["flower", "flow", "flight"]
print(longest_common_prefix_binary(strs))  # Output: "fl"
```

---

### Go Implementation

#### Character-by-Character Check

```go
package main

import (
	"fmt"
	"strings"
)

func longestCommonPrefix(strs []string) string {
	if len(strs) == 0 {
		return ""
	}

	prefix := ""
	minLen := len(strs[0])
	for _, str := range strs {
		if len(str) < minLen {
			minLen = len(str)
		}
	}

	for i := 0; i < minLen; i++ {
		char := strs[0][i]
		for _, str := range strs {
			if str[i] != char {
				return prefix
			}
		}
		prefix += string(char)
	}
	return prefix
}

func main() {
	strs := []string{"flower", "flow", "flight"}
	fmt.Println(longestCommonPrefix(strs)) // Output: "fl"
}
```

#### Binary Search on Index

```go
package main

import (
	"fmt"
)

func longestCommonPrefixBinary(strs []string) string {
	if len(strs) == 0 {
		return ""
	}

	minLen := len(strs[0])
	for _, str := range strs {
		if len(str) < minLen {
			minLen = len(str)
		}
	}

	isCommonPrefix := func(length int) bool {
		prefix := strs[0][:length]
		for _, str := range strs {
			if len(str) < length || str[:length] != prefix {
				return false
			}
		}
		return true
	}

	left, right := 0, minLen
	for left <= right {
		mid := (left + right) / 2
		if isCommonPrefix(mid) {
			left = mid + 1
		} else {
			right = mid - 1
		}
	}

	return strs[0][:right]
}

func main() {
	strs := []string{"flower", "flow", "flight"}
	fmt.Println(longestCommonPrefixBinary(strs)) // Output: "fl"
}
```

---

Let me know if you’d like any adjustments!