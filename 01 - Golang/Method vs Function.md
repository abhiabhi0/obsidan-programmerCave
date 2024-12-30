**A function** is declared by specifying the types of the arguments, the return values, and the function body.

```go
type Person struct {  
    Name string  
    Age  int  
}

func NewPerson(name string, age int) *Person {  
  return &Person{  
     Name: name,  
     Age:  age,  
  }  
}
```

**A method** is just a function with a receiver argument. It is declared with the same syntax with the addition of the **receiver**.

```go
func (p *Person) isAdult bool {  
  return p.Age > 18  
}
```
