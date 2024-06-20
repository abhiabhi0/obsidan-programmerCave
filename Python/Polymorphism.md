==**single entity in a programming language can behave in multiple ways**== depending on the context.

1. ### Operator Polymorphism
Operator polymorphism, or operator overloading, means that ==**one symbol can be used to perform multiple operations**==

2. ### Function Polymorphism
Python’s built-in `len()` function, for instance, can be used to return the length of an object. However, it will measure the length of the object differently ==**depending on the object’s data type and structure**.==

3. ### Class and Method Polymorphism
parent class establishes the method `type`, so the child class inherits that method. The method is redefined in the child class, however, such that objects instantiated from the child class use the redefined method. This is called ==“**method overriding**.”==


```Python
# Define parent class Animal

class Animal:
# Define method
    def type(self):
        print("animal")

# Define child class Dog    
class Dog (Animal):
    def type(self):
        print("dog")

# Initialize objects
obj_bear = Animal()
obj_terrier = Dog()

obj_bear.type()
# returns animal
obj_terrier.type()
# returns dog
```

4. **Method Overloading**: 
Unlike languages like Java or C++, ==Python doesn't support method overloading by default== (where methods can have the same name but different parameters). However, polymorphic behavior similar to method overloading ==can be achieved using default parameter values or variable argument==s. For example:

```Python
class Calculator:
    def add(self, a, b=0):  # Method overloading using default parameter
        return a + b

    def multiply(self, *args):  # Method overloading using variable arguments
        result = 1
        for arg in args:
            result *= arg
        return result

calc = Calculator()
print(calc.add(5))             # Output: 5
print(calc.add(5, 3))          # Output: 8
print(calc.multiply(2, 3))     # Output: 6
print(calc.multiply(2, 3, 4))  # Output: 24

```

Ref: https://www.educative.io/blog/what-is-polymorphism-python