```go
r1, r2 := func() (string, string) {
    x := []string{"hello", "quickref.me"}
    return x[0], x[1]
}()

// => hello quickref.me
fmt.Println(r1, r2)
```

### Naked Returns
```go
func split(sum int) (x, y int) {
  x = sum * 4 / 9
  y = sum - x
  return
}

x, y := split(17)
fmt.Println(x)   // => 7
fmt.Println(y)   // => 10
```

### Variadic Functions

The function that is called with the varying number of arguments is known as variadic function. Or in other words, a user is allowed to pass zero or more arguments in the variadic function. _fmt.Printf_ is an example of the variadic function, it required one fixed argument at the starting after that it can accept any number of arguments.

```go
function function_name(para1, para2...type)type {// code...}
```

- Inside the function _…type_ behaves like a slice. For example, suppose we have a function signature, i.e, add( b…int)int, now the parameter is of type []int inside the function.
```go
// Variadic function to join strings
func joinstr(elements ...string) string {
	return strings.Join(elements, "-")
}

func main() {

	// zero argument
	fmt.Println(joinstr())

	// multiple arguments
	fmt.Println(joinstr("GEEK", "GFG"))
	fmt.Println(joinstr("Geeks", "for", "Geeks"))
	fmt.Println(joinstr("G", "E", "E", "k", "S"))

}
```

- You can also pass an existing slice in a variadic function. To do this, we pass a slice of the complete array to the function as shown in _Example 2_ below.
```go
// Variadic function to join strings
func joinstr(elements ...string) string {
	return strings.Join(elements, "-")
}

func main() {
	// pass a slice in variadic function
	elements := []string{"geeks", "FOR", "geeks"}
	fmt.Println(joinstr(elements...))
}
```

- When you do not pass any argument in the variadic function, then the slice inside the function is empty.
- The variadic functions are generally used for functions that perform string formatting.
- You can also pass multiple slices in the variadic function.
- You can not use variadic parameter as a return value, but you can return it as a slice.

### Init Function()

import --> const --> var --> init()

```go
var num = setNumber()

func setNumber() int {
    return 42
}
func init() {
    num = 0
}
func main() {
    fmt.Println(num) // => 0
}
```

