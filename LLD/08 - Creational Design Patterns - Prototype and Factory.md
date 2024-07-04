**Prototype**
- is a creational design pattern that can be used to create objects that are similar to each other. 
- The pattern is used to avoid the cost of creating new objects by cloning an
existing object and avoiding dependencies on the class of the object that needs to be cloned.

**Factory**
- is a creational design pattern that can be used to create objects without specifying the exact class of the object that will be created. 
- The pattern is used to avoid dependencies on the class of the object that needs to be created.

---
## Prototype
- allows us to hide the complexity of making new instances from the client.
- The concept is to copy an existing object rather than creating a new instance from scratch, something that may include costly operations. 
- The existing object acts as a prototype and contains the state of the object. The newly copied object may change same properties only if required. 
- This approach saves costly resources and time, especially when object creation is a heavy process.

- Let us say we have to create a new `User` API and we want to test it. To test it, we need to create a new user. We can create a new user by using the `new` keyword.

```java
User user = new User("John", "Doe", "john@doe.in", "1234567890");
```

- We might be calling a separate API to get these random values for the user. So each time we want to create a new user we have to call the API. 
- Instead, we can create a new user by cloning an existing user and modifying the fields that are necessary. This way we can avoid calling the API each time we want to create a new user. 
- To clone an existing user, we have to implement a common interface for all the user objects `clone()`.

```java 
public abstract class User {
	public abstract User clone();
}
 ...
 
User user = new User("John", "Doe", "john@doe.in", "1234567890");
User user2 = user.clone();
user2.setId(2);
```

- Apart from reducing the cost of creating new objects, the prototype pattern also helps in reducing the complexity of creating new objects. 
- The client code does not have to deal with the complexity of creating new objects. It can simply clone the existing object and modify it as per its needs. 
- The client code does not have a dependency on the class of the object that it is cloning.

### Prototype Registry
- prototype pattern can be extended to use a registry of pre-defined prototypes. 
- The registry can be used to store a set of pre-defined prototypes. The client code can then request a clone of a prototype from the registry instead of creating a new object from scratch. 
- The registry can be implemented as a key-value store where the key is the name of the prototype and the value is the prototype object.

**Example**
- we might want to create different types of users. A user with a Student role, a user with a Teacher role, and a user with an Admin role. 
- Each such different type of user might have some fields that are specific to the type so the fields to be copied might be different. 
- We can create a registry of pre-defined prototypes for each of these roles

```java
...

interface UserRegistry {
	User getPrototype(UserRole role);
	void addPrototype(UserRole role, User user);
}

class UserRegistryImpl implements UserRegistry {
	private Map<UserRole, User> registry = new HashMap<>();
 
	@Override
	public User getPrototype(UserRole role) {
		return registry.get(role).clone();
	}
 
	@Override
	public void addPrototype(UserRole role, User user) {
		registry.put(role, user);
	}
}

...

UserRegistry registry = new UserRegistryImpl();
registry.addPrototype(UserRole.STUDENT, new Student("John", "Doe", "john@doe.in", "1234567890", UserRole.STUDENT, "CS"));
User user = registry.getPrototype(UserRole.STUDENT);
user.setId(1);
```

---
## Factory
- is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created. 
- This is done by creating objects by calling a factory method—either specified in an interface and implemented by child classes, or implemented in a base class and optionally overridden by derived classes—rather than by calling a constructor.
- The client code can request an object from a factory object without having to know the class of the object that will be returned. The factory object can create the object and return it to the client code.

### Simple Factory
- is a creational pattern that provides a static method for creating objects.
- The method can be used to create objects without having to specify the exact class of the object that will be created. This is done by creating a factory class that contains a static method for creating objects.

```java
class UserFactory {
	public static User createUser(UserRole role) {
		switch (role) {
			case STUDENT:
				return new Student("John", "Doe");
			case TEACHER:
				return new Teacher("John", "Doe");
			case ADMIN:
				return new Admin("John", "Doe");
		}
	}
}

...

User user = UserFactory.createUser(UserRole.STUDENT);
```

- The complete steps to implement the simple factory pattern are:
	- **Factory class** - Create a factory class that contains a static method for creating objects
	- **Conditional** - Use a conditional statement to create the object based on the input.
	- **Request** - Request an object from the factory class without having to know the class of the object that will be returned
	
### Factory Method
- simple factory method is easy to implement, but it has a few drawbacks. 
- The factory class is not extensible. If we want to add a new type of user, we will have to modify the factory class. 
- Also, the factory class is not reusable. If we want to create a factory for creating different types of objects, we will have to create a new factory class. 
- To overcome these drawbacks, we can use the factory method pattern.

- In the factory method the responsibility of creating the object is shifted to the child classes. 
- The factory method is implemented in the base class and the child classes can override the factory method to create objects of their own type. 
- The factory method is also known as the virtual constructor.

```java 
@AllArgsContructor
abstract class UserFactory {
	public abstract User createUser(String firstName, String lastName);
}

class StudentFactory extends UserFactory {
	@Override
	public User createUser(String firstName, String lastName) {
		return new Student(firstName, lastName);
	}
}

...

UserFactory factory = new StudentFactory();
User user = factory.createUser("John", "Doe");
```

- The complete steps to implement the factory method pattern are:
	- Base factory interface - Create a factory class that contains a method for creating objects.
	- Child factory class - Create a child class that extends the base factory class and overrides the factory method to create objects of its own type.
	- Request - Request an object from the factory class without having to know the class of the object that will be returned.

### Abstract Factory 
- is a creational pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes

**Example**
- We have already created a `User` abstract class. Now we will create
the concrete classes `Student` and `Teacher`. 
- To restrict the usage of subclasses, we can create factories for each of the concrete classes. The `StudentFactory` will be used to create `Student` objects and the `TeacherFactory` will be used to create `Teacher` objects.

```java 
class StudentFactory {
	public User createStudent(String firstName, String lastName) {
		return new Student(firstName, lastName);
	}
}

class TeacherFactory {
	public User createTeacher(String firstName, String lastName) {
		return new Teacher(firstName, lastName);
	}
}

...

StudentFactory studentFactory = new StudentFactory();
Student student = studentFactory.createStudent("John", "Doe");

TeacherFactory teacherFactory = new TeacherFactory();
Teacher teacher = teacherFactory.createTeacher("John", "Doe");
```

- But now we have a problem, we can use the factories to create any type of student and teacher. Should a teacher teaching Physics be able to teach a student of Biology class? 
- This is where the concept of related or a family of objects comes into play. The `Student` and `Teacher` objects are related to each other. A
teacher should only be able to teach a student of the same class. So we can create a factory that can create a family of related objects. The `ClassroomFactory` will be used to create `Student` and `Teacher` objects of the same class.

```java 
abstract class ClassroomFactory {
	public abstract Student createStudent(String firstName, String lastName);
	public abstract Teacher createTeacher(String firstName, String lastName);
}
```

- Now we can create concrete factories for each family of related objects that we want to create.

```java 
class BiologyClassroomFactory extends ClassroomFactory {
	@Override
	public Student createStudent(String firstName, String lastName) {
		return new BiologyStudent(firstName, lastName);
	}
 
	@Override
	public Teacher createTeacher(String firstName, String lastName) {
		return new BiologyTeacher(firstName, lastName);
	}
}
```

- The class `ClassroomFactory` is an abstract class that contains the factory methods for creating the objects. 
- The child classes can override the factory methods to create objects of their own type. The client code can request an object from the factory class without having to know the class of the object that will be returned.

```java 
ClassroomFactory factory = new BiologyClassroomFactory();
Student student = factory.createStudent("John", "Doe");
Teacher teacher = factory.createTeacher("John", "Doe");
```

- The class `ClassroomFactory` becomes our abstract factory that essentially is a factory of factories.

#### Advantages of Abstract Factory
- **Isolate concrete classes** - The client code is not coupled to the concrete classes of the objects that it creates.
- **Easy to exchange product families** - The client code can request an object from the factory class without having to know the class of the object that will be returned. This makes it easy to exchange product families.
- **Promotes consistency among products** - The client code can request an object from the factory class without having to know the class of the object that will be returned. This makes it easy to maintain consistency among products.




