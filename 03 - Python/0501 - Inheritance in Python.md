- **Definition**:
  - Inheritance is a fundamental concept in object-oriented programming that allows a new class (child class) to inherit attributes and methods from an existing class (parent class).
  - This promotes code reusability and establishes a relationship between classes.

### Types of Inheritance in Python

1. **Single Inheritance**:
   - A child class inherits from only one parent class.
   - **Example**:
     ```python
     class Parent:
         def greet(self):
             return "Hello from Parent!"

     class Child(Parent):
         def welcome(self):
             return "Welcome from Child!"

     child_instance = Child()
     print(child_instance.greet())  # Output: Hello from Parent!
     print(child_instance.welcome()) # Output: Welcome from Child!
     ```

2. **Multiple Inheritance**:
   - A child class inherits from more than one parent class.
   - Allows the child class to access methods and attributes from multiple parent classes.
   - **Example**:
     ```python
     class Mother:
         def mother_name(self):
             return "Mother: Sita"

     class Father:
         def father_name(self):
             return "Father: Ram"

     class Child(Mother, Father):
         def child_name(self):
             return "Child: Anu"

     child_instance = Child()
     print(child_instance.mother_name())  # Output: Mother: Sita
     print(child_instance.father_name())  # Output: Father: Ram
     ```

3. **Multilevel Inheritance**:
   - A child class inherits from a parent class, which in turn inherits from another parent class.
   - Forms a chain of inheritance.
   - **Example**:
     ```python
     class Grandparent:
         def grandparent_name(self):
             return "Grandparent"

     class Parent(Grandparent):
         def parent_name(self):
             return "Parent"

     class Child(Parent):
         def child_name(self):
             return "Child"

     child_instance = Child()
     print(child_instance.grandparent_name())  # Output: Grandparent
     ```

4. **Hierarchical Inheritance**:
   - Multiple child classes inherit from a single parent class.
   - Each child can have its own unique methods while sharing the parent's methods.
   - **Example**:
     ```python
     class Parent:
         def parent_name(self):
             return "Parent"

     class Child1(Parent):
         def child1_name(self):
             return "Child 1"

     class Child2(Parent):
         def child2_name(self):
             return "Child 2"

     child1_instance = Child1()
     child2_instance = Child2()
     print(child1_instance.parent_name())  # Output: Parent
     print(child2_instance.parent_name())  # Output: Parent
     ```

5. **Hybrid Inheritance**:
   - A combination of two or more types of inheritance.
   - Can involve multiple inheritance and multilevel inheritance together.
   - **Example**:
     ```python
     class Base:
         def base_method(self):
             return "Base Method"

     class Derived1(Base):
         def derived1_method(self):
             return "Derived Class 1 Method"

     class Derived2(Base):
         def derived2_method(self):
             return "Derived Class 2 Method"

     class Hybrid(Derived1, Derived2):
         def hybrid_method(self):
             return "Hybrid Class Method"

     hybrid_instance = Hybrid()
     print(hybrid_instance.base_method())      # Output: Base Method
     print(hybrid_instance.derived1_method())  # Output: Derived Class 1 Method
     ```

### Accessing Parent Functions or Members in a Child Class

- To access a method or attribute of the parent class within the child class, you can use the following approaches:

1. **Direct Access**:
   - You can directly call the parent method using the `self` keyword.
   - Example:
   ```python
   class Parent:
       def show(self):
           return "This is the Parent method."

   class Child(Parent):
       def display(self):
           return self.show()  # Accessing parent method

   child_instance = Child()
   print(child_instance.display())  # Output: This is the Parent method.
   ```

2. **Using `super()` Function**:
   - The `super()` function allows you to call methods from the parent class without explicitly naming it, which is useful for maintaining code flexibility during inheritance changes.
   - Example:
   ```python
   class Parent:
       def greet(self):
           return "Hello from Parent!"

   class Child(Parent):
       def greet(self): 
           return super().greet() + " and Child!"  # Accessing parent method

   child_instance = Child()
   print(child_instance.greet())  # Output: Hello from Parent! and Child!
   ```

### Summary

- Inheritance allows for creating a new class based on an existing one, promoting code reuse and establishing relationships between classes.
- There are five main types of inheritance in Python: single, multiple, multilevel, hierarchical, and hybrid.
- You can access parent functions or members in a child class using direct access or the `super()` function.

These notes provide a comprehensive overview of inheritance types in Python and how to access parent functions or members in a child class, preparing you well for your interview.

Citations:
[1] https://pythonlobby.com/inheritance-and-types-of-inheritance-in-python-programming/
[2] https://www.geeksforgeeks.org/types-of-inheritance-python/
[3] https://www.geeksforgeeks.org/inheritance-in-python/
[4] https://www.boardinfinity.com/blog/types-of-inheritance-in-python/
[5] https://www.javatpoint.com/types-of-inheritance-python