## Constructor
- special method that is called when an object is created. 
- used to initialize the object. 
- called automatically when the object is created. 
- used to set initial values for object attributes.
-  **it's a method, but it has no return type**
- **implicitly returns the type of the object that it creates**
##### Syntax of a constructor:
- In Java, Constructor declarations begin with access modifiers: They can be public, private, protected, or package access, based on other access modifiers. 
- Unlike methods, a constructor can't be abstract, static, final, native, or synchronized.

### Types of Constructor:
#### Default constructor:
- constructor created by the compiler if we do not define any constructor(s) for a class. 
#### Parameterized constructor:
-  real benefit of constructors is that they help us maintain encapsulation when injecting state into the object.

---
## Access modifiers
- There are two types of modifiers in Java: **access modifiers** and **non-access modifiers**.
### Types of access modifiers in Java:
- **public** - The access level of a public modifier is everywhere. It can be accessed from within the class, outside the class, within the package and outside the package
- **protected** - The access level of a protected modifier is within the package and outside the package through child class. If you do not make the child class, it cannot be accessed from outside the package. 
- **private** - The access level of a private modifier is only within the class. It cannot be accessed from outside the class.
- **default** - The access level of a default modifier is only within the package. It cannot be accessed from outside the package. If you do not specify any access level, it will be the default.

| Modifier  | Class | Package | Subclass | Global |
| --------- | ----- | ------- | -------- | ------ |
| Public    | Yes   | Yes     | Yes      | Yes    |
| Protected | Yes   | Yes     | Yes      | No     |
| Default   | Yes   | Yes     | No       | No     |
| Private   | Yes   | No      | No       | No     |

---
## Inheritance:
-  mechanism that allows one class to acquire all the properties from another class by inheriting the class.
- represents the IS-A relationship which is also known as a parent-child relationship.

### Types of inheritance:
 - **Single** - when a class can have only one parent class.
 - **Multilevel** - when a class can have multiple parent classes at different levels.
 - **Hierarchical** - When two or more classes inherits a single class, it is known as hierarchical inheritance.
 - **Multiple** - When a class can have multiple parent classes, it is known as multiple inheritance.

---
## Diamond Problem

- ambiguity that arises when two classes B and C inherit from A, and class D inherits from both B and C.
-  If there is a method in A that B and C have overridden, and D does not override it, then which version of the method does D inherit: that of B, or that of C.

![[Pasted image 20240622221813.png]]

---

### Java Code

user.java
```java
package com.scaler.lld.scaler;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private String email;

    public void changeEmail(String email) {
        this.email = email;
    }

    public void printInfo() {
    }

    public void printInfo(String title) {
        System.out.println(" \n User: " + title + " " + this.getName());
    }
}

// Interfaces
// Class - Blueprint
// Interface - Blue print of behaviour
// Database db = new MySqlDB();
// Interfaces - define methods with an impl.
// Abstract - mixture of interface and class
// implemented methods + not-implemented
// Abstract classes - state

```