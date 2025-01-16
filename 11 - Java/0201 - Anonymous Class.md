An **anonymous class** is a local class without a name and is used to instantiate classes or interfaces. It allows you to make your code more concise by defining a class that will be used only once. Anonymous classes are particularly useful in scenarios where you want to override methods of a class or implement an interface without creating a separate named class.

#### Key Features of Anonymous Classes
1. **No Name**: As the name suggests, anonymous classes do not have a name.
2. **Single Instantiation**: They can be instantiated only once.
3. **Defined Inside a Method**: Typically declared inside a method or a code block.
4. **No Constructor**: Since they have no name, they cannot have constructors.
5. **Access to Outer Class Members**: They can access the members (including private members) of the enclosing class.
6. **Cannot be Static**: Anonymous classes cannot be static.

#### Syntax
The syntax for creating an anonymous class is similar to that of creating an instance of a class, but it includes the definition of the class body in curly braces:

```java
new SuperclassOrInterface() {
    // Fields, methods, and constructors go here
};
```

### Example of Anonymous Class

#### 1. Implementing an Interface

Here’s an example demonstrating how to create an anonymous class that implements an interface:

```java
interface Greeting {
    void greet();
}

public class Main {
    public static void main(String[] args) {
        // Creating an anonymous class implementing Greeting interface
        Greeting greeting = new Greeting() {
            public void greet() {
                System.out.println("Hello from the anonymous class!");
            }
        };
        greeting.greet(); // Output: Hello from the anonymous class!
    }
}
```

#### 2. Extending a Class

You can also create an anonymous class that extends another class:

```java
class Animal {
    void sound() {
        System.out.println("Animal makes sound");
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating an anonymous class extending Animal
        Animal animal = new Animal() {
            void sound() {
                System.out.println("Dog barks");
            }
        };
        animal.sound(); // Output: Dog barks
    }
}
```

### Diagrammatic Representation

Here's a simple diagram illustrating the concept of an anonymous class:

```
+---------------------+
|     Outer Class     |
|                     |
|  +---------------+  |
|  |   Anonymous   |  |
|  |     Class     |  |
|  +---------------+  |
|                     |
+---------------------+
```

In this diagram, the outer class contains the definition of the anonymous class, which is created without a name.

### Differences Between Regular and Anonymous Inner Classes

| Feature                       | Regular Inner Class              | Anonymous Inner Class         |
|-------------------------------|----------------------------------|-------------------------------|
| Name                          | Has a name                      | No name                       |
| Instantiation                 | Can be instantiated multiple times | Can be instantiated only once |
| Constructor                    | Can have constructors            | Cannot have constructors       |
| Access                        | Can access outer class members   | Can also access outer class members |
| Implementation                | Can implement multiple interfaces | Can implement only one interface |

### Use Cases
- **Event Handling**: Commonly used in GUI applications to handle events.
- **Thread Creation**: Often used to create threads with specific behavior.

### Conclusion
Anonymous classes provide a convenient way to implement interfaces or extend classes without cluttering your code with additional named classes. They are particularly useful for short-lived tasks where you need to override methods or implement interfaces quickly.

This summary should serve as effective preparation notes for understanding and discussing anonymous classes in Java during interviews.

Citations:
[1] https://takeuforward.org/java/java-anonymous-class/
[2] https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html
[3] https://www.prepbytes.com/blog/java/anonymous-class-in-java/
[4] https://www.youtube.com/watch?v=n2Dpffp_HLc
[5] https://www.geeksforgeeks.org/anonymous-inner-class-java/
[6] https://www.scientecheasy.com/2020/06/anonymous-inner-class-in-java.html/