- ability of a message to be represented in many forms.
- Polymorphism in Java can be achieved in two ways i.e., **method overloading** and **method overriding**.
- Polymorphism in Java is mainly divided into two types.
   -  **Compile-time polymorphism**
   -  **Runtime polymorphism**
- Compile-time polymorphism can be achieved by method overloading
- Runtime polymorphism can be achieved by method overriding.

![[Polymorphism.excalidraw]]

### Subtyping
- concept in object-oriented programming that allows a variable of a base class to reference a derived class object. This is called polymorphism, because the variable can take on many forms.
- The variable can be used to call methods that are defined in the base class, but the actual implementation of the method is defined in the derived class.
- Eg
```java
public class User {
	private String name;
	private String email;
}

public class Student extends User {
	private String batchName;
	private Integer psp;
}

User user = new Student();
```

### Method Overloading (Compile time polymorphism)
- feature that allows a class to have more than one method having the same name, if their argument lists are different. 
- It is similar to constructor overloading in Java, that allows a class to have more than one constructor having different argument lists.
```java
public class User {
	private String name;
	private String email;
	
	public void printUser() {
		System.out.println("Name: " + name + ", Email: " + email);
	}
	
	public void printUser(String name, String email) {
		System.out.println("Name: " + name + ", Email: " + email);
	}
}
```

- The compiler distinguishes these two methods by the number of parameters in the list and their data types. The return type of the method does not matter.

### Method Overloading (Runtime polymorphism)
- Runtime polymorphism is also called Dynamic method dispatch. Instead of resolving the overridden method at compile-time, it is resolved at runtime.
```java
public class User {
	private String name;
	private String email;

	public void printUser() {
		System.out.println("Name: " + name + ", Email: " + email);
	}
}

public class Student extends User {
	private String batchName;
	private Integer psp;

	@Override
	public void printUser() {
		System.out.println("Name: " + name + ", Email: " + email + ",
		Batch: " + batchName + ", PSP: " + psp);
	}
}
```

#### Advantages of Polymorphism
- Code reusability is the main advantage of polymorphism; once a class is defined, it can be used multiple times to create an object.

#### Problems with Polymorphism
- Implementing code is complex because understanding the hierarchy of classes and its overridden method is quite difficult.
- Runtime polymorphism can lead to the real-time performance issue (during the process), it basically degrades the performances as decisions are taken at run time because, machine needs to decide which method or variable to invoke

### Interface
- similar to a class, but it cannot be instantiated. 
- It can contain only constants, method signatures, default methods, static methods, and nested types. 
- Method bodies exist only for default methods and static methods.
-  can be thought of as a blueprint of behavior. 
- It is used to achieve abstraction and multiple inheritance in Java.

#### Why use an interface?
- It is used to achieve abstraction.
- Due to multiple inheritance, it can achieve loose coupling. 
- Define a common behavior for unrelated classes.

```java
public interface Person {
	String getName();
	String getEmail();
}

public class User implements Person {
	private String name;
	private String email;
	
	public User(String name, String email) {
		this.name = name;
		this.email = email;
	}
	
	@Override
	public String getName() {
		return name;
	}
	
	@Override
	public String getEmail() {
		return email;
	}
}
```
