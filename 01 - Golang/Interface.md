An interface type in Go is kind of like a _definition_. It defines and describes the exact methods that _some other type_ must have.

One example of an interface type from the standard library is the [`fmt.Stringer`](https://pkg.go.dev/fmt/#Stringer) interface, which looks like this:

```go
type Stringer interface {     
	String() string 
} 
```

For example, the following `Book` type satisfies the interface because it has a `String() string` method:

```go
type Book struct {
    Title  string
    Author string
}

func (b Book) String() string {
    return fmt.Sprintf("Book: %s - %s", b.Title, b.Author)
}
```

another example, the following `Count` type _also_ satisfies the `fmt.Stringer` interface — again because it has a method with the exact signature `String() string`.

```go
type Count int

func (c Count) String() string {
    return strconv.Itoa(int(c))
}
```

```go
func WriteLog(s fmt.Stringer) {
    log.Print(s.String())
}
```
Because this `WriteLog()` function uses the `fmt.Stringer` interface type in its parameter declaration, we can pass in any object that satisfies the `fmt.Stringer` interface. For example, we could pass either of the `Book` and `Count` types that we made earlier to the `WriteLog()` method, and the code would work OK.

```go
// Declare a WriteLog() function which takes any object that satisfies
// the fmt.Stringer interface as a parameter.
func WriteLog(s fmt.Stringer) {
    log.Print(s.String())
}

func main() {
    // Initialize a Count object and pass it to WriteLog().
    book := Book{"Alice in Wonderland", "Lewis Carrol"}
    WriteLog(book)

    // Initialize a Count object and pass it to WriteLog().
    count := Count(3)
    WriteLog(count)
}
```

### Why are they useful?
1. To help reduce duplication or boilerplate code.
2. To make it easier to use mocks instead of real objects in unit tests.
3. As an architectural tool, to help enforce decoupling between parts of your codebase.