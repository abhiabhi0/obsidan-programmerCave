```go
func scope() func() int{
    outer_var := 2
    foo := func() int {return outer_var}
    return foo
}

// Outpus: 2
fmt.Println(scope()())
```

```go
func outer() (func() int, int) {
    outer_var := 2
    inner := func() int {
        outer_var += 99
        return outer_var
    }
    inner()
    return inner, outer_var
}
inner, val := outer()
fmt.Println(inner()) // => 200
fmt.Println(val)     // => 101
```
