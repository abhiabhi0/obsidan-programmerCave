### Definition of Polymorphism

- **Meaning**:
  - The term "polymorphism" comes from Greek, meaning "many forms."
  - In programming, it refers to the ability of a single interface to represent different underlying forms (data types).

- **In Python**:
  - Polymorphism allows methods and functions to behave differently based on the input type or the object that calls them.

### Types of Polymorphism in Python

1. **Function Polymorphism**:
   - Functions can accept arguments of different types and perform operations accordingly.
   - Example with built-in functions:
     ```python
     print(len("Hello"))      # Output: 5 (string)
     print(len([1, 2, 3]))    # Output: 3 (list)
     ```
   - The `len()` function works with various data types, demonstrating polymorphic behavior.

2. **Operator Overloading**:
   - Operators can be used with different types of operands.
   - Example:
     ```python
     num1 = 10
     num2 = 20
     print(num1 + num2)       # Output: 30 (addition of integers)

     str1 = "Hello"
     str2 = "World"
     print(str1 + " " + str2) # Output: Hello World (concatenation of strings)
     ```
   - The `+` operator behaves differently based on whether it is used with numbers or strings.

3. **Method Overriding**:
   - Inheritance allows a child class to provide a specific implementation of a method that is already defined in its parent class.
   - Example:
     ```python
     class Animal:
         def sound(self):
             return "Some sound"

     class Dog(Animal):
         def sound(self):
             return "Bark"

     class Cat(Animal):
         def sound(self):
             return "Meow"

     def animal_sound(animal):
         print(animal.sound())

     dog = Dog()
     cat = Cat()
     animal_sound(dog)  # Output: Bark
     animal_sound(cat)  # Output: Meow
     ```
   - Here, the `sound()` method is overridden in both `Dog` and `Cat` classes.

4. **Duck Typing**:
   - Python uses duck typing, which means that the type or class of an object is less important than the methods it defines or the way it behaves.
   - If an object behaves like a certain type (i.e., has the necessary methods), it can be treated as that type.
   - Example:
     ```python
     class Bird:
         def fly(self):
             return "Flying"

     class Airplane:
         def fly(self):
             return "Flying high"

     def let_it_fly(flyable):
         print(flyable.fly())

     bird = Bird()
     airplane = Airplane()
     let_it_fly(bird)      # Output: Flying
     let_it_fly(airplane)  # Output: Flying high
     ```

### Advantages of Polymorphism

- **Code Reusability**: Allows for writing generic code that can work with different data types.
- **Flexibility and Maintainability**: Makes it easier to extend and maintain code without modifying existing functionality.
- **Improved Readability**: Code can be written more clearly and concisely.

### Summary

- Polymorphism in Python enables functions and methods to operate on different data types seamlessly.
- It can be achieved through function overloading, operator overloading, method overriding, and duck typing.
- Understanding polymorphism is essential for effective programming in Python, especially when working with OOP concepts.

These notes provide a comprehensive overview of polymorphism in Python, including its definition, types, advantages, and practical examples. This should help you prepare effectively for your interview.

Citations:
[1] https://www.geeksforgeeks.org/polymorphism-in-python/
[2] https://www.shiksha.com/online-courses/articles/all-about-polymorphism-in-python/
[3] https://www.simplilearn.com/polymorphism-in-python-article
[4] https://www.javatpoint.com/polymorphism-in-python
[5] https://www.programiz.com/python-programming/polymorphism
[6] https://www.youtube.com/watch?v=tHN8I_4FIt8