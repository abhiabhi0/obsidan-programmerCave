<mark class="hltr-g">Instead of a class creating its own dependencies, they are provided to the class, making the system more modular, testable, and maintainable.</mark>

### Without DI
```java
// Engine.java
public class Engine {
    public void start() {
        System.out.println("Engine started.");
    }
}

// Car.java
public class Car {
    private Engine engine;

    public Car() {
        this.engine = new Engine(); // Dependency is created inside the class
    }

    public void drive() {
        engine.start();
        System.out.println("Car is driving.");
    }

    public static void main(String[] args) {
        Car car = new Car();
        car.drive();
    }
}
```
