### 1. **Function Literals**

A **function literal** is an **anonymous function** in Go. It allows defining a function inline and using it as a value. This is useful for closures or when passing functions as arguments.

#### Example:

```go
package main

import "fmt"

func main() {
    // Function literal assigned to a variable
    add := func(a, b int) int {
        return a + b
    }
    fmt.Println(add(3, 5)) // Output: 8

    // Inline function literal
    result := func(x int) int {
        return x * x
    }(4) // Immediately invoked
    fmt.Println(result) // Output: 16
}
```

---

### 2. **Naked Returns**

In Go, you can **omit the explicit return values** if the function has named return variables. This is called a **naked return**.

#### Example:

```go
package main

import "fmt"

func sumAndProduct(a, b int) (sum int, product int) {
    sum = a + b      // Assign values to named return variables
    product = a * b
    return           // Naked return
}

func main() {
    sum, product := sumAndProduct(3, 5)
    fmt.Println("Sum:", sum, "Product:", product)
}
```

**Key Points:**

- Naked returns can make the code concise but may reduce readability in longer functions.
- Use them sparingly in short, clear functions.

---

### 3. **Variadic Functions**

A **variadic function** can take a variable number of arguments of the same type. In Go, variadic parameters are declared using `...` before the type.

#### Example:

```go
package main

import "fmt"

// A function to sum all numbers
func sum(nums ...int) int {
    total := 0
    for _, num := range nums {
        total += num
    }
    return total
}

func main() {
    fmt.Println(sum(1, 2, 3))        // Output: 6
    fmt.Println(sum(5, 10, 15, 20)) // Output: 50
}
```

**Key Points:**

- Variadic parameters must be the last parameter in the function.
- You can pass a slice using the `...` syntax:
    
    ```go
    nums := []int{1, 2, 3}
    fmt.Println(sum(nums...)) // Output: 6
    ```
    

---

### 4. **Init Functions**

The **`init` function** is a special function in Go that is executed automatically when a package is imported or when the program starts. It is used for setup tasks like initializing variables or checking conditions.

#### Rules for `init` Functions:

- Every package can have one or more `init` functions.
- `init` functions do not take arguments or return values.
- They are called automatically before the `main` function runs.

#### Example:

```go
package main

import "fmt"

// Global variables
var count int

func init() {
    // Initialize or configure global state
    fmt.Println("Init function called")
    count = 42
}

func main() {
    fmt.Println("Main function called")
    fmt.Println("Count:", count) // Output: 42
}
```

#### Use Case in Packages:

```go
// file: config.go
package config

import "fmt"

var AppName string

func init() {
    AppName = "MyApp"
    fmt.Println("Config initialized")
}

// file: main.go
package main

import (
    "fmt"
    "config"
)

func main() {
    fmt.Println("App Name:", config.AppName)
}
```

---

### Summary Table

|Concept|Description|Key Features|
|---|---|---|
|**Function Literal**|Inline, anonymous functions used as values or closures.|Enables immediate use or passing functions as arguments.|
|**Naked Returns**|Allows omitting explicit return values when named return variables are declared.|Improves brevity in simple functions; less readable in complex cases.|
|**Variadic Functions**|Functions that accept a variable number of arguments of the same type.|Use `...type` syntax; last parameter only; supports passing slices with `...`.|
|**Init Functions**|Special functions automatically called to initialize package-level variables or configurations.|No parameters or return values; executed once before `main` or package-level code.|

Would you like these concepts with examples added to your Obsidian notes?