- **Definition of OOP**:
  - Object-Oriented Programming is a programming paradigm that organizes code into objects, which are instances of classes.
  - It models real-world entities and their interactions, allowing for modular and reusable code.

- **Core Concepts of OOP**:
  1. **Class**:
     - A blueprint for creating objects; defines attributes (data) and methods (functions).
     - Example:
       ```python
       class Dog:
           def __init__(self, name, age):
               self.name = name
               self.age = age
           def bark(self):
               return "Woof!"
       ```

  2. **Object**:
     - An instance of a class that has state (attributes) and behavior (methods).
     - Example:
       ```python
       my_dog = Dog("Buddy", 3)
       print(my_dog.bark())  # Output: Woof!
       ```

  3. **Method**:
     - A function defined within a class that operates on the attributes of the object.
     - Example:
       ```python
       def get_age(self):
           return self.age
       ```

  4. **Inheritance**:
     - The mechanism by which one class can inherit attributes and methods from another class.
     - Promotes code reusability and establishes a relationship between classes.
     - Example:
       ```python
       class Puppy(Dog):  # Inherits from Dog
           def play(self):
               return "Playing!"
       ```

  5. **Polymorphism**:
     - The ability to present the same interface for different underlying data types.
     - Allows methods to do different things based on the object calling them.
     - Example:
       ```python
       def animal_sound(animal):
           print(animal.bark())

       dog = Dog("Rex", 5)
       animal_sound(dog)  # Output: Woof!
       ```

  6. **Encapsulation**:
     - Bundling data (attributes) and methods that operate on the data into a single unit (class).
     - Restricts direct access to some of an object's components, which is a means of preventing unintended interference.
     - Example using private attributes:
       ```python
       class BankAccount:
           def __init__(self, balance):
               self.__balance = balance  # Private attribute

           def deposit(self, amount):
               self.__balance += amount

           def get_balance(self):
               return self.__balance
       ```

  7. **Abstraction**:
     - Hiding complex implementation details and showing only the essential features of an object.
     - Achieved through abstract classes or interfaces in Python.
     - Example using abstract base class:
       ```python
       from abc import ABC, abstractmethod

       class Animal(ABC):
           @abstractmethod
           def sound(self):
               pass

       class Cat(Animal):
           def sound(self):
               return "Meow"
       ```

- **Advantages of OOP**:
  - **Modularity**: Code is organized into separate classes, making it easier to manage.
  - **Reusability**: Inheritance allows for reusing existing code without modification.
  - **Scalability**: Easier to scale applications by adding new classes or modifying existing ones.
  - **Maintainability**: Encapsulation and abstraction improve code maintainability.

- **Comparison with Procedure-Oriented Programming**:
  | Feature                     | OOP                             | Procedure-Oriented Programming |
  |-----------------------------|---------------------------------|-------------------------------|
  | Focus                       | Objects and classes             | Functions and procedures      |
  | Data Handling               | Data is encapsulated in objects | Data is separate from functions|
  | Reusability                 | High (through inheritance)      | Low                           |
  | Complexity Management        | Better management of complexity   | Harder to manage complexity    |

These notes encapsulate the essential aspects of OOP in Python, providing you with a comprehensive overview for your interview preparation.

Citations:
[1] https://www.javatpoint.com/python-oops-concepts
[2] https://realpython.com/python3-object-oriented-programming/
[3] https://www.geeksforgeeks.org/python-oops-concepts/
[4] https://www.freecodecamp.org/news/how-to-use-oop-in-python/
[5] https://www.datacamp.com/tutorial/python-oop-tutorial