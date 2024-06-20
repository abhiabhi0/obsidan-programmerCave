### Read multiple lines from console
```go
func main() {
    fmt.Println("input text:")
    var w1, w2, w3 string
    n, err := fmt.Scan(&w1, &w2, &w3)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("number of items read: %d\n", n)
    fmt.Printf("read text: %s %s %s-\n", w1, w2, w3)
}
```

Output:

```shell
input text:
ab
cd
ef
number of items read: 3
read text: ab cd ef-
```

> With the [`fmt.Scan()`](https://pkg.go.dev/fmt#Scan) you can read multiple lines of text only if the line consists of a single word.

### Read a single character from terminal
```go
func main() {
    fmt.Println("input text:")
    var char rune
    _, err := fmt.Scanf("%c", &char)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("read character: %c-\n", char)
}
```

Output:

```shell
input text:
abcd
read character: a-
```

### Read formatted user input
```go
func main() {
    fmt.Println("input text:")
    var name string
    var country string
    n, err := fmt.Scanf("%s is born in %s", &name, &country)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("number of items read: %d\n", n)
    fmt.Println(name, country)
}
```

```shell
input text:
Anna is born in Germany 
number of items read: 2
Anna Germany
```

### Read numbers from user input

#### Use [`fmt.Scanf()`](https://pkg.go.dev/fmt#Scanf) to read a number

```go
package main

import (
    "fmt"
    "log"
)

func main() {
    fmt.Println("input number:")
    var number int64
    _, err := fmt.Scanf("%d", &number)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("read number: %d\n", number)
}
```

Output:

```shell
input number:
8
read number: 8
```

#### Use [`fmt.Scan()`](https://pkg.go.dev/fmt#Scan) to read a number

```go
package main

import (
    "fmt"
    "log"
)

func main() {
    fmt.Println("input number:")
    var number int64
    _, err := fmt.Scan(&number)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("read number: %d\n", number)
}
```

Output:

```shell
input number:
8
read number: 8
```

