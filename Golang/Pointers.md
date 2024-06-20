```go
func main () {
  b := *getPointer()
  fmt.Println("Value is", b)
}

func getPointer () (myPointer *int) {
  a := 234
  return &a
}

a := new(int)
*a = 234
```
