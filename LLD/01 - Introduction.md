### Low-level design (LLD):
- component-level design process that follows a step-by-step refinement process. 
- used for designing data structures, required software architecture, source code and ultimately, performance algorithms. 
- Overall, the data organization may be defined during requirement analysis and then refined during data design work. 
- Post-build, each component is specified in detail

#### Why LLD?
- goal of LLD or a low-level design (LLD) is to give the **internal logical design of the actual program code**.
- describes the class diagrams with the methods and relations between classes and program specs. 
- It describes the modules so that the programmer can directly code the program from the document.
- Ultimately, LLD has the following goals:
  - **Low level implementation details of a system**
  - **organization of code**
  - **write good software**
- A good software is a software that is
  - easy to maintain
  - easy to scale.
  - easy to extend

##### Maintainability:
-  is a long-term aspect that describes how easily software can evolve and change, which is especially important in todayʼs agile environment.
- ISO 25010 states that a highly maintainable software system must possess the following qualities:
  - **Modularity** - The product is composed of discrete components such that a change to one component has minimal impact on other components.
  - **Reusability** - The product makes use of assets that can be re-used in building other assets or in other systems.
  - **Analyzability** - The impact of an intended change on the product, diagnosis of deficiencies, causes of failures or identification of the components that need to be changed can be analyzed effectively and efficiently.
  - **Modifiability** - The product can be effectively and efficiently modified without introducing defects or degrading existing product quality.
  - **Testability** - The test criteria can be established effectively and efficiently, and the product can be tested to determine whether those criteria have been met.
##### Scalability:
- is an attribute of a tool or a system to increase its capacity and functionalities based on its usersʼ demand. 
- Scalable software can remain stable while adapting to changes, upgrades, overhauls, and resource reduction.

##### Extensibility:
- is a measure of the ability to extend a system and the level of effort required to implement the extension. 
- can be through the addition of new functionality or through modification of existing functionality. 
- The principle provides for enhancements without impairing existing system functions
---
## Programming paradigms
### Imperative:
- an imperative program consists of commands for the computer to perform to change state e.g. C, Java, Python, etc.
#### Procedural programming:
- programming paradigm that uses a sequence of steps to solve a problem.
- based on the concept of the procedure call. 
- Procedures (a type of routine or subroutine) simply contain a series of computational steps to be carried out. 
- Any given procedure might be called at any point during a program's execution, including by other procedures or itself.
- **Think of all programming as managing the relationship between two fundamental concepts: state and behavior. State is the data of your program. Behavior is the logic.**
- State is held in data structures. Behavior is held in functions (also known as procedures or subroutines). A procedural application therefore passes data structures into functions to produce some output.
- Eg: Imagine you want to transfer some money from one account to another. These are the following steps that you would take:
  - Open the source account
  - Withdraw the money
  - Open the destination account
  - Deposit the money in destination account
  - A procedural version of this program would be:
  ```python
 def transfer(source: int, destination: int, amount: int) -> None:
    source_account = get_account(source)
    update_account(source_account, -amount)
    destination_account = get_account(destination)
    update_account(destination_account, amount)

def get_account(number: int) -> dict:
    return list(filter(lambda account: account['number'] == number,accounts))[0]

def update_account(account: int, delta: int) -> None:
    account['balance'] += delta
	```


#### Object Oriented Programming 
[[02 - Object-oriented programming (OOP)]]]

### Declarative:
- focuses on what the program should accomplish without specifying all the details of how the program should achieve the result e.g. SQL, Lisp, etc.




