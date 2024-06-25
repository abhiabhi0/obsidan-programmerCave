## Single Responsibility Principle
- states that a class should do one thing, and therefore it should have only a single reason to change.
![[Pasted image 20240625174445.png]]

### Case study - Design a bird
- A bird could have the following attributes:
	- Weight
	- Colour
	- Type
	- Size
	- BeakType
- A bird would also exhibit the following behaviours:
	- Fly
	- Eat
	- Make a sound
```mermaid
classDiagram
    class Bird{
        +weight: int
        +colour: string
        +type: string
        +size: string
        +beakType: string
        +fly()
        +eat()
        +makeSound()
    }
```

```java
public class Bird {
	private int weight;
	private String colour;
	private String type;
	private String size;
	private String beakType;
	
	public void fly() {
		...
	}
	
	public void eat() {
		...
	}
	
	public void makeSound() {
		...
	}
}
```

- Since each bird has a different method of flying, we would have to implement conditional statements to check the type of the bird and then call the appropriate method.
```java
public void fly() {
	if (type.equals("eagle")) {
		flyLikeEagle();
	} else if (type.equals("penguin")) {
		flyLikePenguin();
	} else if (type.equals("parrot")) {
		flyLikeParrot();
	}
}
```

- The above code exhibits the following problems:
  - **Readability** - The code is not readable. It is difficult to understand what the code is doing.
  - **Testing** - It is difficult to test the code. We would have to test each type of bird separately
  - **Reusability** - The code is not reusable. If we want to re-use the code of specific type of bird, we would have to change the above code.
  - **Parallel development** - The code is not parallel development friendly. If multiple developers are working on the same code, they could face merge conflicts.
  - **Multiple reasons to change** - The code has multiple reasons to change. If we want to change the way a type of bird flies, we would have to change the code in the fly method

#### Reasons to follow SRP
- overcoming the problems mentioned above
- **Maintainability** - Smaller, well-organized classes are easier to search than monolithic ones. 
- **Ease of testing** – A class with one responsibility will have far fewer test cases. 
- **Lower coupling** – Less functionality in a single class will have fewer dependencies

#### How/Where to spot violations of SRP
- multiple if-else statements
- Monster methods or God classes - Methods that are too long and doing much more than the name suggests.
```java
public saveToDatabase() { 
	// Connect to database 
	// Create a query 
	// Execute the query
	// Create a user defined object 
	// Close the connection
}
```

---
## Open/Closed Principle

- states that a class should be open for extension but closed for modification. This means that we should be able to **add new functionality to the class without changing the existing code**.
```java
public void fly() {
	if (type.equals("eagle")) {
		flyLikeEagle();
	} else if (type.equals("penguin")) {
		flyLikePenguin();
	} else if (type.equals("parrot")) {
		flyLikeParrot();
	}
}
```
- In the above code, we are checking the type of the bird and then calling the appropriate method. 
- If we want to add a new type of bird, we would have to change the code in the fly method. 
- This is a violation of the Open/Closed Principle

### Abstract classes and interfaces
- class that is declared using the abstract keyword. 
- allows us to declare method signatures using the abstract keyword (abstract method) and forces its subclasses to implement all the declared methods. 
- if a class has a method that is abstract, then the class itself must be abstract.
- Abstract classes have no restrictions on field and method modifiers, while in an interface, all are public by default
- Abstract classes may also have constructors which will get executed during the child object's instantiation.
- An abstract class can have abstract methods and non-abstract methods.
- An abstract method is a method that is declared without an implementation. It is a method that is declared using the abstract keyword and does not have a body.

#### When to use abstract classes and interfaces?
- If multiple classes have common functionalities, we would like to use inheritance to avoid code duplication and also have fixed contracts so that the subclasses are forced to implement the common functionalities. 
- If the common classes have common attributes, consider using abstract classes since they can have instance variables. 
- If the common classes have common methods, consider using interfaces since they can have only abstract methods. However, the implementation of the methods can be different in the subclasses. Interfaces are also useful when we want to have multiple inheritance

### Fixing OCP violation in the Bird class
- In order to fix the SRP violations, we would consider having a parent class `Bird` and child classes `Eagle`, `Penguin`, and `Parrot`.
- Since, different birds have the same attributes and behaviours, we would want to use classes
- An instance of the `Bird` class does not make sense, hence we would use an abstract class. 
- We can't use an interface since we would want to have instance variables

```mermaid
classDiagram
    Bird <|-- Eagle
    Bird <|-- Penguin
    Bird <|-- Parrot
    class Bird{
        +weight: int
        +colour: string
        +type: string
        +size: string
        +beakType: string
        +fly()
    }
    class Eagle{
        +fly()
    }
    class Penguin{
        +fly()
    }
    class Parrot{
        +fly()
    }
```


