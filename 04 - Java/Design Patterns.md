1. **Singleton Pattern**:
   - **Explanation**: Ensures that a class has only one instance and provides a global point of access to that instance.
   - **Example**: A logger class that needs to be instantiated only once throughout the application.
   ```java
   public class Logger {
       private static Logger instance;

       private Logger() {}

       public static Logger getInstance() {
           if (instance == null) {
               instance = new Logger();
           }
           return instance;
       }

       public void log(String message) {
           System.out.println(message);
       }
   }
   ```

2. **Factory Method Pattern**:
   - **Explanation**: Defines an interface for creating an object, but allows subclasses to alter the type of objects that will be created.
   - **Example**: A factory for creating different types of shapes.
   ```java
   interface Shape {
       void draw();
   }

   class Circle implements Shape {
       @Override
       public void draw() {
           System.out.println("Drawing Circle");
       }
   }

   class Rectangle implements Shape {
       @Override
       public void draw() {
           System.out.println("Drawing Rectangle");
       }
   }

   class ShapeFactory {
       public Shape createShape(String type) {
           if (type.equalsIgnoreCase("circle")) {
               return new Circle();
           } else if (type.equalsIgnoreCase("rectangle")) {
               return new Rectangle();
           }
           return null;
       }
   }
   ```

3. **Observer Pattern**:
   - **Explanation**: Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
   - **Example**: A weather station that notifies multiple displays when the weather changes.
   ```java
   import java.util.ArrayList;
   import java.util.List;

   interface Observer {
       void update(String weather);
   }

   class WeatherStation {
       private List<Observer> observers = new ArrayList<>();
       private String weather;

       public void addObserver(Observer observer) {
           observers.add(observer);
       }

       public void removeObserver(Observer observer) {
           observers.remove(observer);
       }

       public void setWeather(String weather) {
           this.weather = weather;
           notifyObservers();
       }

       private void notifyObservers() {
           for (Observer observer : observers) {
               observer.update(weather);
           }
       }
   }

   class Display implements Observer {
       @Override
       public void update(String weather) {
           System.out.println("Weather update: " + weather);
       }
   }
   ```

4. **Decorator Pattern**:
   - **Explanation**: Attaches additional responsibilities to an object dynamically, providing a flexible alternative to subclassing for extending functionality.
   - **Example**: Adding toppings to a pizza dynamically.
   ```java
   interface Pizza {
       String getDescription();
       double getCost();
   }

   class BasicPizza implements Pizza {
       @Override
       public String getDescription() {
           return "Pizza";
       }

       @Override
       public double getCost() {
           return 5.0;
       }
   }

   class PizzaDecorator implements Pizza {
       private Pizza pizza;

       public PizzaDecorator(Pizza pizza) {
           this.pizza = pizza;
       }

       @Override
       public String getDescription() {
           return pizza.getDescription();
       }

       @Override
       public double getCost() {
           return pizza.getCost();
       }
   }

   class CheeseDecorator extends PizzaDecorator {
       public CheeseDecorator(Pizza pizza) {
           super(pizza);
       }

       @Override
       public String getDescription() {
           return super.getDescription() + ", Cheese";
       }

       @Override
       public double getCost() {
           return super.getCost() + 1.0;
       }
   }
   ```

These examples illustrate how popular design patterns can be applied in Java to solve common software design challenges, enhancing code modularity, flexibility, and maintainability.