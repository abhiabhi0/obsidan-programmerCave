Type assertion in Go is a way to retrieve the underlying value of an interface variable. It allows you to convert an interface to its actual concrete type.

### **Syntax**:

```go
value, ok := interfaceVariable.(TargetType)
```

- `interfaceVariable`: The variable of the interface type.
- `TargetType`: The type you are asserting the value to be.
- `value`: The asserted value.
- `ok`: A boolean indicating whether the assertion succeeded.

---

### **Example of Type Assertion**:

```go
package main

import "fmt"

func main() {
	var i interface{} = 42

	// Type assertion
	value, ok := i.(int)
	if ok {
		fmt.Printf("Value is an int: %d\n", value)
	} else {
		fmt.Println("Assertion failed!")
	}

	// Attempting an incorrect type assertion
	str, ok := i.(string)
	if ok {
		fmt.Printf("Value is a string: %s\n", str)
	} else {
		fmt.Println("Assertion failed! i is not a string.")
	}
}
```

**Output**:

```
Value is an int: 42
Assertion failed! i is not a string.
```

---

### **Panic on Failed Type Assertion**:

If you omit the `ok` check and the assertion fails, it will cause a panic.

```go
package main

func main() {
	var i interface{} = 42
	// This will panic because `i` is not a string
	str := i.(string)
	fmt.Println(str)
}
```

**Output**:

```
panic: interface conversion: interface {} is int, not string
```

---

### **Practical Use Case**:

Type assertion is often used when working with interfaces like `interface{}` or custom interfaces where the exact type is not known upfront.

#### Example: Handling Different Types in a Function

```go
package main

import "fmt"

func describe(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Value is an int: %d\n", v)
	case string:
		fmt.Printf("Value is a string: %s\n", v)
	default:
		fmt.Printf("Value is of unknown type: %T\n", v)
	}
}

func main() {
	describe(42)
	describe("hello")
	describe(3.14)
}
```

**Output**:

```
Value is an int: 42
Value is a string: hello
Value is of unknown type: float64
```

---

### **Key Points**:

1. **Single Value**: If you are sure of the type and don't need to check success:
    
    ```go
    value := interfaceVariable.(TargetType)
    ```
    
    Be cautious as this will panic if the type assertion fails.
    
2. **Two Values**: Use the `ok` form to avoid panic:
    
    ```go
    value, ok := interfaceVariable.(TargetType)
    ```
    
3. **Use in Switch Statements**: Type assertions are frequently used in `type switch` to handle multiple types.
    
4. **Common Use Case**: Often used with `interface{}` or when dealing with Go's `reflect` package.