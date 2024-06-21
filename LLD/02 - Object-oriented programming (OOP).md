
- programming paradigm that uses objects to model real-world things and aims to implement state and behavior using objects.
- State and behaviour are combined into one new concept: an Object. An OO application can therefore produce some output by calling an Object, without needing to pass data structures.
- Advantages of OO include the potential for information hiding: if a caller needn't pass any data structure, then the caller needn't be aware of any data structure, and can therefore be completely decoupled from the data format.
##### Abstraction:
- process of hiding the implementation details of a program from the user.
##### Encapsulation:
- used to hide the values or state of a structured data object inside a class,
- preventing direct access to them by clients in a way that could expose hidden implementation details or violate state invariance maintained by the methods.
##### Class:
- is a blueprint which you use to create objects.
##### Object:
- is an instance of a class.
Eg:
```java
public class OopBankAccount {
    private Integer number;
    private Integer balance;
    
    public OopBankAccount(Integer number, Integer balance) {
        this.number = number;
        this.balance = balance;
    }
    
    void deposit(Integer amount) {
        this.balance += amount;
    }
    
    void withdraw(Integer amount) {
        this.balance += amount;
    }
    
    void transfer(OopBankAccount destination, Integer amount) {
        this.withdraw(amount);
        destination.deposit(amount);
    }
}
```

###### Advantages:
- **Reusability**: Through classes and objects, and inheritance of common attributes and functions.
- **Security**: Hiding and protecting information through encapsulation.
- **Maintenance**: Easy to make changes without affecting existing objects much.
- **Inheritance**: Easy to import required functionality from libraries and customize them, thanks to inheritance.

###### Disadvantages:
 - Beforehand planning of entities that should be modeled as classes.
 - OOPS programs are usually larger than those of other paradigms.
- [Banana-gorilla problem](https://dev.to/efpage/what-s-wrong-with-the-gorilla-2l4j#:~:text=Joe%20Armstrong%2C%20the%20principal%20inventor,and%20the%20entire%20jungle.%22.) - You wanted a banana but what you got was a gorilla holding the banana and the entire jungle