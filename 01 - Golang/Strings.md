```go
s1 := "Hello" + "World"

s2 := `A "raw" string literal
can include line breaks.`

// Outputs: 10
fmt.Println(len(s1))

// Outputs: Hello
fmt.Println(string(s1[0:5]))
```

### function examples

|   |   |
|---|---|
|Contains("test", "es")|true|
|Count("test", "t")|2|
|HasPrefix("test", "te")|true|
|HasSuffix("test", "st")|true|
|Index("test", "e")|1|
|Join([]string{"a", "b"}, "-")|a-b|
|Repeat("a", 5)|aaaaa|
|Replace("foo", "o", "0", -1)|f00|
|Replace("foo", "o", "0", 1)|f0o|
|Split("a-b-c-d-e", "-")|[a b c d e]|
|ToLower("TEST")|test|
|ToUpper("test")|TEST|
