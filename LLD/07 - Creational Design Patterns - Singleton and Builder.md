**Design Pattern**
- is a general, reusable solution to a commonly occurring problem within a given context in software design. 
- are formalized best practices that the programmer can use to solve common problems when designing an application or system.

**Creational Design Patterns**
- provide various object creation mechanisms, which increase flexibility and reuse of existing code.

**Singleton**
- is a software design pattern that restricts the instantiation of a class to one object.

**Builder**
-  lets you construct complex objects step by step.
- allows you to produce different types and representations of an object using the same construction code.

## Singleton

### Problems
- **Shared resource**
	- Imagine you have a class that is responsible for managing the database connection. You want to make sure that only one instance of this class exists in your application. If you create multiple instances of this class, you will end up with multiple database connections, which is not what you want. 
	- Similarly, there can be a class that is responsible for managing the logging mechanism. You want to make sure that only one instance of this class exists in your application. If you create multiple instances of this class, you will end up with multiple log files, which is not what you want.
	
- **Single access point**
	- Applications often require configuration. For example, you might want to configure the database connection parameters. You want to make sure that only one instance of this class exists in your application. A configuration class should have a single access point to the configuration parameters. If you create multiple instances of this class, you will end up with multiple configuration files.

### Solution

- is a **creational design pattern** that lets you ensure that a class has only one instance, while providing a global access point to this instance. 
- To implement the Singleton patter, the following steps are required:
  - **Constructor hiding**: 
	- The constructor of the singleton class should be private or protected. 
	- This will prevent other classes from instantiating the singleton class.
  - **Global access point**:
	- The singleton class should provide a global access point to get the instance of the singleton class. 
	- This global access point should be static and should return the same instance of the singleton class every time it is called. 
	- If the instance does not exist, it should create the instance and then return it.
	
#### Simple Singleton in Java
- first step is to hide the constructor by making it private. This will prevent other classes from instantiating the singleton class.

```java
public class Database {
	private Database() {
	}
}
```

- above code restricts the instantiation of the Database class. Now, we need to provide a global access point to get the instance of the Database class.
- We can do this by creating a static method that returns the instance of the Database class. If the instance does not exist, it should create the instance and then return it.

```java
public class Database {
	private static Database instance = new Database();
	
	private Database() {
	}
	
	public static Database getInstance() {
		if (instance == null) {
			instance = new Database();
		}

		return instance;
	}
}
```

#### Thread-Safe Singleton in Java
- above code is not thread-safe. 
- If two threads call the getInstance() method at the same time, both threads will check if the instance variable is null. Both threads will find that the instance variable is null. 
- Both threads will create a new instance of the Database class. This will result in two instances of the Database class. 

```java
public class Database {
	private static Database instance = new Database();
	
	private Database() {
	}
	
	public static synchronized Database getInstance() {
		if (instance == null) {
			instance = new Database();
		}

		return instance;
	}
}
```

#### Double-Checked Locking in Java
- above code is thread-safe. However, it is not efficient. 
- If two threads call the getInstance() method at the same time, both threads will check if the instance variable is null. Both threads will find that the instance variable is null. Both threads will wait for the lock to be released. 
- Once the lock is released, one thread will create a new instance of the Database class. The other thread will wait for the lock to be released. Once the lock is released, it will create a new instance of the Database class. This will result in two instances of the Database class. 
- To make the above code efficient, we can use double-checked locking.

```java
public class Database {
	private static Database instance = new Database();
	
	private Database() {
	}
	
	public static Database getInstance() {
		if (instance == null) {
			synchronized (Database.class) {
				if (instance == null) {
					instance = new Database();
				}
			}
		}

		return instance;
	}
}
```

#### Singleton Pattern in Python

```python
class ConnectionPool(object):
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            print("Creating the object")
            cls._instance = super(ConnectionPool, cls).__new__(cls)
        return cls._instance
```

- The object is created on the first call to the class:
```python
pool1 = ConnectionPool()
print(pool1)
```

```
Creating the object
<__main__.ConnectionPool object at 0x7f15ecf9fe80>
```

- But the second call returns the same instance. The message “Creating the object” does not print, nor is a different object returned:

```python
pool2 = ConnectionPool()
print(pool2)
print("Are they the same object?", pool1 is pool2)
```

```
<__main__.ConnectionPool object at 0x7f15ecf9fe80>
Are they the same object? True
```

#### Singleton Pattern in Golang
##### Using user-defined function

```go 
var lock = &sync.Mutex{}

type connectionPool struct {
}

var connPoolInstance *connectionPool

func GetInstance() *connectionPool {
    if connPoolInstance == nil {
        lock.Lock()
        defer lock.Unlock()
        if connPoolInstance == nil {
            fmt.Println("Creating Connection Pool Instance Now")
            connPoolInstance = &connectionPool{}
        } else {
            fmt.Println("Connection Pool Instance already created-1")
        }
    } else {
        fmt.Println("Connection Pool Instance already created-2")
    }
    return connPoolInstance
}
```

- Above code ensures that only one instance of the `connectionPool` struct is created.
- There is a check at the start for nil `connPoolInstance`. This is to prevent the expensive lock operations every time `Getinstance()` method is called. If this check fails then it means that `connPoolInstance` is already created
- The `connPoolInstance` is created inside the lock.
- There is another check for nil `connPoolInstance` after the lock is acquired. This is to make sure that if more than one goroutine bypass the first check then only one goroutine is able to create the singleton instance otherwise each of the goroutine will create its own instance of the single struct.

##### Using sync.Once 
- `sync.Once` will only perform the operation only once

```go
var once sync.Once

type connectionPool struct {
}

var connPoolInstance *connectionPool

func GetInstanceUsingSync() *connectionPool {
    if connPoolInstance == nil {
        once.Do(
            func() {
                fmt.Println("Creating Connection Pool Instance Now")
                connPoolInstance = &connectionPool{}
            })
    } else {
        fmt.Println("Connection Pool Instance already created-2")
    }
    return connPoolInstance
}
```


## Builder

### Problems
- **Complex object creation**
	- There are multiple ways to create an object, but constructors are the primary technique used for creating instances of a class. However, constructors become unmanageable when there is a need to create an object with many parameters. This is known as the **telescoping constructor anti-pattern. 
	- The telescoping constructor anti-pattern is a code smell that indicates that the class has too many constructors. This is a code smell because it is difficult to maintain and extend the class.
	
- **Validation and failing object creation**
	- There are cases when you want to validate the parameters before creating an object. For example, you might want to validate the parameters before creating a database connection. If the parameters are invalid, you might want to throw an exception.
	- However, if we use the default constructor, we cannot fail object creation
	
- **Immutability** 
	- Mutable objects are objects whose state can be changed after they are created. Immutable objects are objects whose state cannot be changed after they are created. Immutable objects are easier to maintain and extend whereas mutable objects can lead to bugs. 
	- However, if we use the default constructor, we cannot create immutable objects.
	
### Solution

#### Inner Class
- we can use a class to accept parameters for object creation. The parameter class is type safe, and it is easy to identify the parameters.

```java
public class Database {
	private String host;
	private int port;
	private String username;
	private String password;
 
	public Database(DatabaseParameters parameter) {
		this.host = parameter.host;
		this.port = parameter.port;
		this.username = parameter.username;
		this.password = parameter.password;
	}
}

class DatabaseParameters {
	public String host;
	public int port;
	public String username;
	public String password;
}
```

- above code is type safe. However, it is not easy to use. 
- We need to create an instance of the `DatabaseParameters` class and then pass it to the Database class. This is not a good approach because it is difficult to maintain and extend the code. 
- Similarly, if we even want to change a single parameter name, we have to open the database class for modification.
-  Instead, we should move the destructuring of the parameter class and validation logic to the Parameter class. 
- This will require creating a Database constructor with all the fields. Again, why would developers not just want to use the constructor instead? - So we need a way to allow the parameter class to create the Database object while not exposing a constructor.
-This can be done using an inner class. This inner class is known as the **builder class**.

```java
public class Database {
	private String host;
	private int port;
	private String username;
	private String password;
	 
	private Database() {
	}
	
	public static class DatabaseBuilder {
		private String host;
		private int port;
		private String username;
		private String password;
	 
		public Database build() {
			Database database = new Database();
			database.host = this.host;
			database.port = this.port;
			database.username = this.username;
			database.password = this.password;
			return database;
		}
	}
}
```

- above code now allows us to create a Database object using the DatabaseBuilder class. 
- We can fail object creation by adding a validation hook to the build method. The objects created are immutable because the Database class does not have any setters. And the developer can create objects with any permutation of parameters.

```java 
Database database = new Database.DatabaseBuilder()
	.host("localhost")
	.port(3306)
	.username("root")
	.password("password")
	.build();
```

#### Builder Pattern in Python 

```python 
from __future__ import annotations
from abc import ABC, abstractmethod

class Builder(ABC):
    @abstractmethod
    def with_name(self, name: str) -> Builder:
        pass

    @abstractmethod
    def with_url(self, host: str, port: int) -> Builder:
        pass

    @abstractmethod
    def build(self) -> Database:
        pass

class DatabaseBuilder(Builder):
    def __init__(self) -> None:
        self._database = Database()

    def with_name(self, name: str) -> DatabaseBuilder:
        self._database._name = name
        return self
    
    def with_url(self, host: str, port: int) -> DatabaseBuilder:
        self._database._host = host
        self._database._port = port
        return self
    
    def build(self) -> Database:
        if not self.is_valid():
            raise ValueError("Invalid database configuration")
        return self._database
    
    def is_valid(self) -> bool:
        return self._database._name is not None


class Database:
    def __init__(self, name=None, host=None, port=None):
        self._name = name
        self._host = host
        self._port = port

    def __repr__(self):
        return f"Database(name={self._name}, host={self._host}, port={self._port})"
    
if __name__ == "__main__":
    try:
        # Building a valid database
        db = DatabaseBuilder().with_name("MyDB").with_url("localhost", 3306).build()
        print(db)

        # Building an invalid database (missing name)
        invalid_db = DatabaseBuilder().with_url("localhost", 3306).build()
        print(invalid_db)
    except ValueError as e:
        print(e)
```

#### Builder pattern in Golang

```go
package builder

import (
	"errors"
)

type Database struct {
	name string
	host string
	port int
}

type DatabaseBuilder struct {
	database *Database
}

func NewDatabaseBuilder() *DatabaseBuilder {
	return &DatabaseBuilder{database: &Database{}}
}

func (b *DatabaseBuilder) WithName(name string) *DatabaseBuilder {
	b.database.name = name
	return b
}

func (b *DatabaseBuilder) WithUrl(host string, port int) *DatabaseBuilder {
	b.database.host = host
	b.database.port = port
	return b
}

func (b *DatabaseBuilder) Build() (*Database, error) {
	if !b.isValid() {
		return nil, errors.New("invalid database configuration")
	}
	return b.database, nil
}

func (b *DatabaseBuilder) isValid() bool {
	return b.database.name != ""
}
```

**References:**
[Singleton Pattern in Python](https://python-patterns.guide/gang-of-four/singleton/)
[Singleton Pattern in Golang](https://golangbyexample.com/singleton-design-pattern-go/)
[Github](https://github.com/abhiabhi0/design-patterns)

