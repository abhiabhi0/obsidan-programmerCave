- ability of a message to be represented in many forms.
- Polymorphism in Java can be achieved in two ways i.e., **method overloading** and **method overriding**.
- Polymorphism in Java is mainly divided into two types.
   -  **Compile-time polymorphism**
   -  **Runtime polymorphism**
- Compile-time polymorphism can be achieved by method overloading
- Runtime polymorphism can be achieved by method overriding.

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

### Method Overriding (Compile time polymorphism)
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

### Method Overriding (Runtime polymorphism)
- Runtime polymorphism is also called Dynamic method dispatch. Instead of resolving the overridden method at compile-time, it is resolved at runtime