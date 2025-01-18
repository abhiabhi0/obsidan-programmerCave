Slices in Go are a flexible, convenient, and powerful abstraction for sequences of elements. They are built on top of arrays and provide a more dynamic way to work with collections of data.

Internally, a slice is a descriptor of a segment of an underlying array, consisting of three components:

- **Pointer**: Points to the first element of the slice in the array.
- **Length**: The number of elements in the slice.
- **Capacity**: The maximum number of elements the slice can grow to without reallocating.

```go
package main

import "fmt"

func main() {
	var a = []int{10, 20, 30, 40, 50}
	b := a[:3]
	fmt.Println(b) // [10 20 30]
	fmt.Println(len(b)) // 3
	fmt.Println(cap(b)) // 5
}
```

```
Underlying Array: [10, 20, 30, 40, 50]
Slice Header: {
  Pointer: -> [10, 20, 30, 40, 50],
  Length: 3,
  Capacity: 5
}
```

Here, the slice describes the first 3 elements of the array but has the capacity to grow up to 5 elements without reallocation.

---

## Key Properties of Slices

1. **Slices Do Not Have Fixed Lengths**: A slice’s length is dynamic and can grow or shrink during runtime, as opposed to arrays, which have a fixed size.
2. **Shared Underlying Arrays**: Multiple slices can share the same underlying array. Changes to one slice are reflected in others sharing the same array.
3. **Value Semantics**: Assigning one slice to another copies the slice header, not the underlying array. Thus, slices act as a lightweight view of the underlying data.

---

## How Slices Differ from Arrays

- **Arrays**:
    - Have fixed size: `[5]int` is distinct from `[3]int`.
    - Are value types: Assigning one array to another copies the entire array.

Example:

```go
package main
import "fmt"

func main() {
    var a [5]int
    b := a
    b[2] = 7
    fmt.Println(a, b) // prints [0 0 0 0 0] [0 0 7 0 0]
}
```

- **Slices**:
    - Have dynamic lengths and capacities.
    - Share the same underlying array when assigned.

Example:

```go
package main
import "fmt"

func main() {
    a := []int{1, 2, 3, 4, 5}
    b := a[2:]
    b[0] = 0
    fmt.Println(a, b) // prints [1 2 0 4 5] [0 4 5]
}
}
```

---

## The Slice Header

A slice is represented internally by a struct, often referred to as the slice header:

```go
package runtime

type slice struct {
    ptr   unsafe.Pointer
    len   int
    cap   int
}
```

When a slice is passed to a function, its header is copied, but the underlying array is not. This behavior can lead to confusion, as seen in the following example:

```go
package main
import "fmt"

func double(s []int) {
    s = append(s, s...)
}

func main() {
    s := []int{1, 2, 3}
    double(s)
    fmt.Println(s, len(s)) // prints [1 2 3] 3
}
```

The slice `s` inside `double` is an independent copy of the slice header from `main`. Changes to the copy do not affect the original slice in `main`.

---

## Operations on Slices

### 1. Appending Elements

The `append` function dynamically grows a slice. If the slice exceeds its capacity, a new array is allocated.

```go
slice := []int{1, 2, 3}
slice = append(slice, 4, 5)
fmt.Println(slice) // Output: [1, 2, 3, 4, 5]
```

Appending one slice to another:

```go
slice1 := []int{1, 2, 3}
slice2 := []int{4, 5, 6}
slice1 = append(slice1, slice2...)
fmt.Println(slice1) // Output: [1, 2, 3, 4, 5, 6]
```

### 2. Copying Slices

Use the `copy` function to copy elements between slices. The number of elements copied is the minimum of the lengths of the source and destination slices.

```go
src := []int{1, 2, 3}
dst := make([]int, len(src))
copy(dst, src)
fmt.Println(dst) // Output: [1, 2, 3]
```

### 3. Using Slices as Stacks

Slices can be used as stacks with `append` and slicing.

Push operation:

```go
stack := []int{}
stack = append(stack, 10) // Push 10
```

Pop operation:

```go
val := stack[len(stack)-1] // Peek
stack = stack[:len(stack)-1] // Pop
```

### 4. Modifying Slices in Functions

Since slices share the underlying array, modifications in functions affect the caller.

Example:

```go
func negate(s []int) {
    for i := range s {
        s[i] = -s[i]
    }
}

func main() {
    a := []int{1, 2, 3}
    negate(a)
    fmt.Println(a) // prints [-1 -2 -3]
}
```

### 5. Inserting Elements

To insert an element, use the `copy` function to shift elements.

```go
func Insert(slice []int, index, value int) []int {
    slice = append(slice, 0) // Extend length
    copy(slice[index+1:], slice[index:])
    slice[index] = value
    return slice
}

slice := []int{1, 2, 3, 4}
slice = Insert(slice, 2, 99)
fmt.Println(slice) // Output: [1, 2, 99, 3, 4]
```

---

## Nil vs. Empty Slices

- A **nil slice** has no underlying array and cannot hold any values.
- An **empty slice** has a length of 0 but may still have capacity to grow.

```go
var nilSlice []int
emptySlice := make([]int, 0)
fmt.Println(nilSlice == nil) // true
fmt.Println(emptySlice == nil) // false
```

---

## Conclusion

Slices in Go provide a dynamic and flexible way to work with collections. Understanding their internal mechanics, such as the slice header, shared underlying arrays, and value semantics, helps write efficient and predictable code. Mastering operations like `append`, `copy`, and slicing is key to leveraging the full power of Go slices.

https://dave.cheney.net/2018/07/12/slices-from-the-ground-up
https://go.dev/blog/slices-intro
https://go.dev/blog/slices