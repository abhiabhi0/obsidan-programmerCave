An **immutable class** in Java is a class whose instances cannot be modified after they are created. This means that once an object of an immutable class is instantiated, its state (i.e., its data) remains constant throughout its lifetime. Immutable objects offer several advantages, including thread safety, ease of caching, and predictable behavior in concurrent environments.

#### Key Characteristics of Immutable Classes

1. **Final Class**: The class should be declared as `final` to prevent it from being subclassed. This ensures that the behavior of the immutable class cannot be altered by inheritance.

2. **Final Fields**: All fields should be declared as `private` and `final`. This prevents the fields from being modified after they are initialized.

3. **No Setter Methods**: Immutable classes should not provide setter methods that allow changing the values of fields.

4. **Constructor Initialization**: All fields must be initialized through the constructor when the object is created.

5. **Defensive Copies**: If the class contains mutable objects (like arrays or collections), it should return defensive copies of these objects to maintain immutability.

6. **Methods Returning New Instances**: Instead of modifying the current instance, methods that appear to change the object's state should return a new instance with the updated values.

#### Example of an Immutable Class

Here’s an example illustrating how to create an immutable class representing a simple 2D point:

```java
public final class ImmutablePoint {
    private final int x;
    private final int y;

    // Constructor to initialize the values
    public ImmutablePoint(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // Getter methods to access the values
    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }

    // Method to create a new instance with updated x value
    public ImmutablePoint withX(int newX) {
        return new ImmutablePoint(newX, this.y);
    }

    // Method to create a new instance with updated y value
    public ImmutablePoint withY(int newY) {
        return new ImmutablePoint(this.x, newY);
    }

    @Override
    public String toString() {
        return "Point(" + x + ", " + y + ")";
    }
}
```

#### Usage of the Immutable Class

```java
public class Main {
    public static void main(String[] args) {
        ImmutablePoint p1 = new ImmutablePoint(10, 20);
        System.out.println(p1); // Output: Point(10, 20)

        // Attempt to modify x and y (will cause compilation error)
        // p1.x = 30; // Compilation error: The final field ImmutablePoint.x cannot be assigned

        // Update the x value using withX method
        ImmutablePoint p2 = p1.withX(30);
        System.out.println(p2); // Output: Point(30, 20)

        // Original point p1 remains unchanged
        System.out.println(p1); // Output: Point(10, 20)
    }
}
```

### Advantages of Using Immutable Classes

1. **Thread Safety**: Since immutable objects cannot change state after creation, they are inherently thread-safe. This eliminates issues related to concurrent modifications.

2. **Caching**: Immutable objects can be safely cached because their state does not change. Once created, they can be reused without worrying about unexpected modifications.

3. **Key in Maps**: Immutable classes can be used as keys in HashMaps and other map data structures because their hash codes and state won't change, ensuring proper key-value associations.

4. **Value Objects**: They are often used to represent value objects (e.g., dates, time, currency) where identity is based on state rather than object identity.

5. **Method Parameters and Return Types**: Using immutable classes as method parameters and return types prevents unintended changes to objects passed into or returned from methods.

6. **Security**: In security-sensitive scenarios, immutable classes can help prevent data tampering and enhance data integrity.

### Conclusion

Immutable classes are a powerful feature in Java that provide numerous benefits related to safety, performance, and ease of use. By ensuring that objects cannot be modified after creation, developers can create more robust applications that are easier to maintain and debug.

This detailed overview serves as comprehensive preparation notes for understanding immutable classes in Java, suitable for interviews or further study on the topic.

Citations:
[1] https://www.linkedin.com/pulse/immutable-objects-classes-reda-elsayied
[2] https://www.digitalocean.com/community/tutorials/how-to-create-immutable-class-in-java
[3] https://stackoverflow.com/questions/3769607/why-do-we-need-immutable-class
[4] https://www.geeksforgeeks.org/mutable-and-immutable-objects-in-java-1/