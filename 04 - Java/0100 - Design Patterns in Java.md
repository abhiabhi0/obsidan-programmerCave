Design patterns are standard solutions to common software design problems. They provide a template for how to solve a problem in various contexts and can enhance code readability and reusability. Here’s a detailed overview of the main categories of design patterns in Java:
### **1. Creational Patterns**
Creational patterns deal with object creation mechanisms, trying to create objects in a manner suitable to the situation.
- **Singleton Pattern**
  - Ensures that a class has only one instance and provides a global point of access to it.
  - Useful in scenarios where a single object is needed to coordinate actions across the system.
  - Implementation involves a private constructor and a static method for obtaining the instance.
```java
class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

- **Factory Method Pattern**
  - Defines an interface for creating an object but lets subclasses alter the type of created objects.
  - Promotes loose coupling by eliminating the need for the client to know about concrete classes.
  - [[08 - Creational Design Patterns - Prototype, Factory Method and Abstract Factory]]
```java
// Abstract Product
abstract class Product {
    abstract void use();
}

// Concrete Products
class ConcreteProductA extends Product {
    void use() {
        System.out.println("Using Product A");
    }
}

class ConcreteProductB extends Product {
    void use() {
        System.out.println("Using Product B");
    }
}

// Abstract Creator
abstract class Creator {
    abstract Product factoryMethod(); // Factory method

    // Other methods can be defined here
    public void someOperation() {
        // Call the factory method to create a product
        Product product = factoryMethod();
        product.use(); // Use the product
    }
}

// Concrete Creators
class ConcreteCreatorA extends Creator {
    Product factoryMethod() {
        return new ConcreteProductA(); // Create and return ConcreteProductA
    }
}

class ConcreteCreatorB extends Creator {
    Product factoryMethod() {
        return new ConcreteProductB(); // Create and return ConcreteProductB
    }
}

// Client code
public class FactoryMethodExample {
    public static void main(String[] args) {
        Creator creatorA = new ConcreteCreatorA();
        creatorA.someOperation(); // Outputs: Using Product A

        Creator creatorB = new ConcreteCreatorB();
        creatorB.someOperation(); // Outputs: Using Product B
    }
}
```

- **Abstract Factory Pattern**
  - Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
  - Useful when the system needs to be independent of how its objects are created.

- **Builder Pattern**
  - Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.
  - Particularly useful for constructing objects with many optional parameters.

- **Prototype Pattern**
  - Creates new objects by copying an existing object, known as the prototype.
  - Useful when object creation is costly or complex.

### **2. Structural Patterns**
Structural patterns deal with object composition and typically help ensure that if one part of a system changes, the entire system doesn’t need to change.

- **Adapter Pattern**
  - Allows incompatible interfaces to work together by converting the interface of a class into another interface that clients expect.
  - Useful when you want to use an existing class but its interface does not match what you need.

- **Decorator Pattern**
  - Adds new functionality to an existing object without altering its structure.
  - Useful for adhering to the Single Responsibility Principle by allowing functionality to be divided into classes with unique concerns.

- **Facade Pattern**
  - Provides a simplified interface to a complex subsystem.
  - Helps reduce dependencies on external code and simplifies interactions with complex systems.

- **Composite Pattern**
  - Composes objects into tree structures to represent part-whole hierarchies.
  - Enables clients to treat individual objects and compositions uniformly.

- **Proxy Pattern**
  - Provides a surrogate or placeholder for another object to control access to it.
  - Useful for lazy initialization, access control, logging, etc.

### **3. Behavioral Patterns**
Behavioral patterns focus on communication between objects, what goes on between objects and how they operate together.

- **Observer Pattern**
  - Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
  - Commonly used in event handling systems.

- **Strategy Pattern**
  - Defines a family of algorithms, encapsulates each one, and makes them interchangeable. The strategy lets the algorithm vary independently from clients that use it.
  - Useful for situations where multiple algorithms can be applied based on varying conditions.

- **Command Pattern**
  - Encapsulates a request as an object, thereby allowing users to parameterize clients with queues, requests, and operations.
  - Facilitates undoable operations by storing commands as objects.

- **State Pattern**
  - Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.
  - Useful for implementing finite state machines.

- **Template Method Pattern**
  - Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps without changing the algorithm's structure.
  
### **Conclusion**
Understanding these design patterns is crucial for Java developers as they provide proven solutions that can enhance code maintainability and scalability. Familiarity with these patterns will also help you articulate your design decisions more effectively during interviews.

Citations:
[1] https://www.whizlabs.com/blog/mastering-java-interview-questions/
[2] https://www.indeed.com/career-advice/interviewing/java-basic-interview-questions
[3] https://www.linkedin.com/pulse/ultimate-guide-cracking-your-java-interview-kranthi-shaik
[4] https://workplace.stackexchange.com/questions/168944/cant-remember-much-from-previous-working-experiences
[5] https://www.linkedin.com/pulse/java-interview-tips-guide-aspiring-developers-ali-mirzajanzadeh
[6] https://in.linkedin.com/in/jeyasinghalex
[7] https://www.udemy.com/course/java-interview-questions-and-answers/
[8] https://www.javatpoint.com/how-to-prepare-for-java-interview