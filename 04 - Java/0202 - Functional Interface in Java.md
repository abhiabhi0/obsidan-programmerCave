A **Functional Interface** in Java is an interface that contains exactly one abstract method. It can have multiple default or static methods but must adhere to the single abstract method (SAM) requirement. Functional interfaces are a key feature introduced in Java 8, enabling the use of lambda expressions and method references, which facilitate functional programming paradigms in Java.

#### Key Characteristics of Functional Interfaces
1. **Single Abstract Method**: A functional interface must have exactly one abstract method.
2. **Default and Static Methods**: It can contain any number of default and static methods.
3. **@FunctionalInterface Annotation**: This annotation is used to indicate that the interface is intended to be a functional interface. It helps prevent accidental addition of abstract methods.
4. **Lambda Expressions**: Functional interfaces can be instantiated using lambda expressions, providing a clear and concise way to implement the interface.

#### Syntax
The syntax for defining a functional interface is similar to that of a regular interface but ensures that only one abstract method is defined.

```java
@FunctionalInterface
interface MyFunctionalInterface {
    void myMethod(); // Single abstract method
    default void defaultMethod() {
        System.out.println("This is a default method.");
    }
    static void staticMethod() {
        System.out.println("This is a static method.");
    }
}
```

### Example of Functional Interface

#### 1. Using Lambda Expression

Here’s an example demonstrating how to create a functional interface and implement it using a lambda expression:

```java
@FunctionalInterface
interface Greeting {
    void sayHello(String name);
}

public class Main {
    public static void main(String[] args) {
        // Using lambda expression to implement the Greeting interface
        Greeting greeting = (name) -> System.out.println("Hello, " + name + "!");
        
        greeting.sayHello("Alice"); // Output: Hello, Alice!
    }
}
```

#### 2. Predefined Functional Interfaces

Java provides several predefined functional interfaces in the `java.util.function` package. Here are some commonly used ones:

- **Function<T, R>**: Represents a function that takes an argument of type T and produces a result of type R.

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<Integer, String> intToString = (num) -> "Number: " + num;
        System.out.println(intToString.apply(5)); // Output: Number: 5
    }
}
```

- **Consumer\<T>**: Represents an operation that accepts a single input argument and returns no result.

```java
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Consumer<String> printConsumer = (str) -> System.out.println(str);
        printConsumer.accept("Hello, Consumer!"); // Output: Hello, Consumer!
    }
}
```

- **Predicate\<T>**: Represents a predicate (boolean-valued function) of one argument.

```java
import java.util.function.Predicate;

public class Main {
    public static void main(String[] args) {
        Predicate<Integer> isEven = (num) -> num % 2 == 0;
        System.out.println(isEven.test(4)); // Output: true
    }
}
```

- **Supplier\<T>**: Represents a supplier of results; it does not take any arguments but returns a value.

```java
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        Supplier<Double> randomSupplier = () -> Math.random();
        System.out.println(randomSupplier.get()); // Output: Random number between 0.0 and 1.0
    }
}
```

### Diagrammatic Representation

Here’s a simple diagram illustrating the concept of functional interfaces:

```
+------------------------+
|   Functional Interface  |
|                        |
| +--------------------+ |
| |   Abstract Method   | | <--- Exactly one abstract method
| +--------------------+ |
|                        |
| +--------------------+ |
| | Default Method      | | <--- Can have multiple default methods
| +--------------------+ |
|                        |
| +--------------------+ |
| | Static Method       | | <--- Can have multiple static methods
| +--------------------+ |
+------------------------+
```

### Conclusion

Functional interfaces are essential for enabling functional programming in Java, allowing developers to write more concise and readable code using lambda expressions. They provide a powerful mechanism for passing behavior as parameters, enhancing code flexibility and reusability.

Understanding functional interfaces will not only help you leverage Java's capabilities effectively but also prepare you well for interviews focusing on modern Java practices.

Citations:
[1] https://www.scaler.com/topics/functional-interface-in-java/
[2] https://www.freecodecamp.org/news/functional-programming-in-java/
[3] https://www.javatpoint.com/java-8-functional-interfaces
[4] https://www.geeksforgeeks.org/functional-interfaces-java/
[5] https://dzone.com/articles/functional-interfaces-in-java