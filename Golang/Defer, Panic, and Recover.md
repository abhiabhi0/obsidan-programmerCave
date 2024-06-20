A **defer statement** pushes a function call onto a list. The list of saved calls is executed after the surrounding function returns. Defer is commonly used to simplify functions that perform various clean-up actions.

```go
func CopyFile(dstName, srcName string) (written int64, err error) {
    src, err := os.Open(srcName)
    if err != nil {
        return
    }

    dst, err := os.Create(dstName)
    if err != nil {
        return
    }

    written, err = io.Copy(dst, src)
    dst.Close()
    src.Close()
    return
}
```

This works, but there is a bug. If the call to os.Create fails, the function will return without closing the source file. This can be easily remedied by putting a call to src.Close before the second return statement, but if the function were more complex the problem might not be so easily noticed and resolved. By introducing defer statements we can ensure that the files are always closed:

```go
func CopyFile(dstName, srcName string) (written int64, err error) {
    src, err := os.Open(srcName)
    if err != nil {
        return
    }
    defer src.Close()

    dst, err := os.Create(dstName)
    if err != nil {
        return
    }
    defer dst.Close()

    return io.Copy(dst, src)
}
```

Defer statements allow us to think about closing each file right after opening it, guaranteeing that, regardless of the number of return statements in the function, the files _will_ be closed.

The behavior of defer statements is straightforward and predictable. There are three simple rules:

1. _A deferred function’s arguments are evaluated when the defer statement is evaluated._

In this example, the expression “i” is evaluated when the Println call is deferred. The deferred call will print “0” after the function returns.

```go
func a() {
    i := 0
    defer fmt.Println(i)
    i++
    return
}
```

2. _Deferred function calls are executed in Last In First Out order after the surrounding function returns._

This function prints “3210”:

```go
func b() {
    for i := 0; i < 4; i++ {
        defer fmt.Print(i)
    }
}
```

3. _Deferred functions may read and assign to the returning function’s named return values._

In this example, a deferred function increments the return value i _after_ the surrounding function returns. Thus, this function returns 2:

```
func c() (i int) {
    defer func() { i++ }()
    return 1
}
```

**Panic** is a built-in function that stops the ordinary flow of control and begins _panicking_. When the function F calls panic, execution of F stops, any deferred functions in F are executed normally, and then F returns to its caller. To the caller, F then behaves like a call to panic. The process continues up the stack until all functions in the current goroutine have returned, at which point the program crashes. Panics can be initiated by invoking panic directly. They can also be caused by runtime errors, such as out-of-bounds array accesses.

**Recover** is a built-in function that regains control of a panicking goroutine. Recover is only useful inside deferred functions. During normal execution, a call to recover will return nil and have no other effect. If the current goroutine is panicking, a call to recover will capture the value given to panic and resume normal execution.