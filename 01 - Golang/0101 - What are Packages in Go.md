- **Definition**: A package is a collection of related source files organized by functionality. Each source file must belong to a package, and packages help in structuring the codebase for better maintainability and reusability.

- **Purpose**:
  - **Modularity**: Packages allow developers to break down code into smaller, manageable pieces, making it easier to understand and maintain.
  - **Reusability**: Code within packages can be reused across different projects or applications, reducing redundancy.
  - **Namespace Management**: Packages provide a way to avoid name collisions by grouping related functions, types, and variables under a specific namespace.

### Key Features of Go Packages

1. **Package Declaration**:
   - Every Go source file starts with a `package` declaration that specifies which package the file belongs to.
   - Example:
     ```go
     package main
     ```

2. **Main Package**:
   - The `main` package is special because it defines the entry point of the application. It must contain a `main` function.
   - Other packages can be imported into the `main` package to use their functionalities.

3. **Importing Packages**:
   - To use another package, you must import it at the beginning of your source file using the `import` keyword.
   - Example:
     ```go
     import "fmt"
     ```

4. **Exported Identifiers**:
   - In Go, identifiers (variables, functions, types) that start with an uppercase letter are exported and accessible from other packages. Lowercase identifiers are unexported and only accessible within the same package.

5. **Organizing Code**:
   - Packages should be designed around single concepts or functionalities (e.g., `http`, `json`, `fmt`). This makes it easier to locate and understand related functionalities.

6. **Directory Structure**:
   - Physically, a package corresponds to a directory containing one or more `.go` source files. All files in the same directory should have the same package name.

### Example of Creating and Using a Custom Package

1. **Creating a Custom Package**:
   - Create a directory named `calculator` and add a file named `calculator.go`.

```go
// calculator/calculator.go
package calculator

// Add function adds two integers
func Add(n1, n2 int) int {
    return n1 + n2
}

// Subtract function subtracts two integers
func Subtract(n1, n2 int) int {
    return n1 - n2
}
```

2. **Using the Custom Package**:
   - In your main application file:

```go
// main.go
package main

import (
    "fmt"
    "path/to/your/calculator" // Update this path based on your directory structure
)

func main() {
    sum := calculator.Add(5, 3)
    diff := calculator.Subtract(5, 3)
    
    fmt.Println("Sum:", sum)
    fmt.Println("Difference:", diff)
}
```

### Summary

- Packages are essential for organizing code in Go, promoting modularity and reusability.
- Every Go program consists of one or more packages, with each package serving as a namespace for its identifiers.
- The `main` package serves as the entry point for applications, while other packages can be created for specific functionalities.
- Properly structuring packages enhances code maintainability and collaboration among developers.

Understanding how to effectively use packages is crucial for developing robust applications in Go.

Citations:
[1] https://www.linkedin.com/pulse/unraveling-packages-imports-golang-comprehensive-guide-singh
[2] https://blog.learngoprogramming.com/definitive-guide-to-go-packages-focus-on-the-fundamentals-to-empower-your-skills-d14aae7cd321?gi=2a4ba50ca306
[3] https://www.geeksforgeeks.org/packages-in-golang/
[4] https://www.codementor.io/@inanc/understanding-go-packages-cgsx1hzrx
[5] https://forum.golangbridge.org/t/what-is-a-package-in-golang-official-explanation-glossary/27661
[6] https://www.programiz.com/golang/packages