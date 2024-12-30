```go
package main

import (
	"fmt"
)

func checkPalin(s string) bool {
	r := []rune(s)
	for i := 0; i < len(r); i++ {
		fmt.Println(r[i])
	}
	return true
}

func main() {
	s := "界世界"
	fmt.Println(checkPalin(s))
}
```

