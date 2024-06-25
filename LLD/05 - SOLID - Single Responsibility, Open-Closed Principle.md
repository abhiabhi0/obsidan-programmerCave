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
- 