### **Factory Patterns Overview**

#### **The Problem with `new`**

- Instantiating concrete classes directly (`new`) creates tight coupling between code and implementations, making it hard to extend or maintain.
- Code becomes fragile when changes or new types (e.g., new pizza styles) require frequent modifications.

#### **Identifying What Varies**

- The process of **creating objects** (like different pizza types) varies and should be separated from the rest of the system (e.g., preparing, baking, cutting, boxing).

---

### **Simple Factory**

- A **Simple Factory** centralizes object creation in one place, reducing coupling between object creation logic and the client.
- **How It Works:**
    - A `SimplePizzaFactory` class contains a `createPizza(type)` method that returns an instance of the desired pizza.
    - The `PizzaStore` calls the factory to get the pizza, instead of directly instantiating it.
- **Advantages:**
    - Centralizes object creation.
    - Makes it easier to manage new or modified classes.
- **Limitation:**
    - Still not a true design pattern; more of a programming idiom.
    ![[{EF3E6DC6-B51A-4E5A-AB94-9785E183DD24}.png]]

---
### **Factory Method Pattern**

- This pattern localizes object creation logic in subclasses, decoupling it from the higher-level client.
- **Key Components:**
    - **Abstract Class (PizzaStore):**
        - Defines a method, `orderPizza()`, which contains the common workflow (prepare, bake, cut, box).
        - Declares an abstract method `createPizza(type)`, leaving the implementation to subclasses.
    - **Concrete Subclasses:**
        - Each subclass (e.g., `NYPizzaStore`, `ChicagoPizzaStore`) implements `createPizza(type)` with region-specific pizza styles.
- **How It Works:**
    - The client decides which `PizzaStore` subclass to use (e.g., `NYPizzaStore` or `ChicagoPizzaStore`).
    - The store creates the appropriate pizza without modifying the parent class or duplicating workflow logic.
- **Advantages:**
    - Adheres to the **Open/Closed Principle**: The system is open to extension but closed to modification.
    - Decouples object creation from client code.
    - Ensures consistent processes (e.g., all pizzas follow the same preparation steps) across all stores.

---

### **Real-World Use Case**

#### **Pizza Store Franchise**

1. **Setup:**
    - Franchise owners use `PizzaStore` subclasses tailored to their region's style (e.g., thin crust for New York, deep dish for Chicago).
2. **Flexibility:**
    - Allows regional customization without altering shared processes or creating rigid dependencies.
3. **Framework:**
    - Each store subclass handles its pizza variations via the `createPizza()` method, while the common preparation logic remains consistent across all stores.

---

### **Benefits of Factory Patterns**

1. **Encapsulation of Object Creation:**
    - Reduces dependency on concrete classes.
2. **Flexibility:**
    - Easy to add new types without changing existing code.
3. **Decoupling:**
    - Clients rely on abstract types (interfaces or abstract classes) instead of specific implementations.