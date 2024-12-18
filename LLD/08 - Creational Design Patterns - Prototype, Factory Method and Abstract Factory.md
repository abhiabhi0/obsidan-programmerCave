**Prototype**
- is a creational design pattern that can be <mark class="hltr-g">used to create objects that are similar to each other</mark>. 
- The pattern is used to avoid the cost of <mark class="hltr-o">creating new objects by cloning an</mark>
<mark class="hltr-o">existing object and avoiding dependencies on the class of the object that needs to be cloned</mark>.

**Factory**
- is a creational design pattern that can be used to create objects without specifying the exact class of the object that will be created. 
- The pattern is used to avoid dependencies on the class of the object that needs to be created.

---
## Prototype
- allows us to hide the complexity of making new instances from the client.
- The concept is to copy an existing object rather than creating a new instance from scratch, something that may include costly operations. 
- The existing object acts as a prototype and contains the state of the object. The newly copied object may change same properties only if required. 
- This approach saves costly resources and time, especially when object creation is a heavy process.

- Let us say we have to create a new `User` API and we want to test it. To test it, we need to create a new user. We can create a new user by using the `new` keyword.

```java
User user = new User("John", "Doe", "john@doe.in", "1234567890");
```

- We might be calling a separate API to get these random values for the user. So each time we want to create a new user we have to call the API. 
- Instead, we can create a new user by cloning an existing user and modifying the fields that are necessary. This way we can avoid calling the API each time we want to create a new user. 
- To clone an existing user, we have to implement a common interface for all the user objects `clone()`.

```java 
public abstract class User {
	public abstract User clone();
}
 ...
 
User user = new User("John", "Doe", "john@doe.in", "1234567890");
User user2 = user.clone();
user2.setId(2);
```

- Apart from reducing the cost of creating new objects, the prototype pattern also helps in reducing the complexity of creating new objects. 
- The client code does not have to deal with the complexity of creating new objects. It can simply clone the existing object and modify it as per its needs. 
- The client code does not have a dependency on the class of the object that it is cloning.

### Prototype Registry
- prototype pattern can be extended to use a registry of pre-defined prototypes. 
- The registry can be used to store a set of pre-defined prototypes. The client code can then request a clone of a prototype from the registry instead of creating a new object from scratch. 
- The registry can be implemented as a key-value store where the key is the name of the prototype and the value is the prototype object.

**Example**
- we might want to create different types of users. A user with a Student role, a user with a Teacher role, and a user with an Admin role. 
- Each such different type of user might have some fields that are specific to the type so the fields to be copied might be different. 
- We can create a registry of pre-defined prototypes for each of these roles

```java
...

interface UserRegistry {
	User getPrototype(UserRole role);
	void addPrototype(UserRole role, User user);
}

class UserRegistryImpl implements UserRegistry {
	private Map<UserRole, User> registry = new HashMap<>();
 
	@Override
	public User getPrototype(UserRole role) {
		return registry.get(role).clone();
	}
 
	@Override
	public void addPrototype(UserRole role, User user) {
		registry.put(role, user);
	}
}

...

UserRegistry registry = new UserRegistryImpl();
registry.addPrototype(UserRole.STUDENT, new Student("John", "Doe", "john@doe.in", "1234567890", UserRole.STUDENT, "CS"));
User user = registry.getPrototype(UserRole.STUDENT);
user.setId(1);
```

### Prototype Pattern in Python

```python
from abc import ABC, abstractmethod
from copy import deepcopy
from enum import Enum

class ObjectClonable(ABC):
    @abstractmethod
    def clone(self):
        pass

class MLModel(ObjectClonable):
    def __init__(self, model_type, description, training_split, validation_split, alpha, beta):
        self._model_type = model_type
        self._description = description
        self._training_split = training_split
        self._validation_split = validation_split
        self._alpha = alpha
        self._beta = beta

    def clone(self):
        return deepcopy(self)

    # Getter and Setter methods
    @property
    def model_type(self):
        return self._model_type

    @model_type.setter
    def model_type(self, model_type):
        self._model_type = model_type

    @property
    def description(self):
        return self._description

    @description.setter
    def description(self, description):
        self._description = description

    @property
    def training_split(self):
        return self._training_split

    @training_split.setter
    def training_split(self, training_split):
        self._training_split = training_split

    @property
    def validation_split(self):
        return self._validation_split

    @validation_split.setter
    def validation_split(self, validation_split):
        self._validation_split = validation_split

    @property
    def alpha(self):
        return self._alpha

    @alpha.setter
    def alpha(self, alpha):
        self._alpha = alpha

    @property
    def beta(self):
        return self._beta

    @beta.setter
    def beta(self, beta):
        self._beta = beta

    def __str__(self):
        return f'MLModel(type={self._model_type}, description={self._description}, ' \
               f'training_split={self._training_split}, validation_split={self._validation_split}, ' \
               f'alpha={self._alpha}, beta={self._beta})'

class ModelType(Enum):
    LR = "LR"
    SVM = "SVM"
    DT = "DT"

# Step 4: Create and populate registry
class ModelRegistry:
    def __init__(self):
        self._models = {}

    def register_model(self, model):
        self._models[model.model_type] = model

    def get_model(self, model_type):
        model = self._models.get(model_type)
        return model.clone() if model else None

# Example usage
if __name__ == "__main__":
    # Create models
    lr_model = MLModel(ModelType.LR, "Linear Regression Model", 0.7, 0.3, 0.01, 0.1)
    svm_model = MLModel(ModelType.SVM, "Support Vector Machine Model", 0.6, 0.4, 0.02, 0.2)

    # Register models
    registry = ModelRegistry()
    registry.register_model(lr_model)
    registry.register_model(svm_model)

    # Retrieve and clone models
    cloned_lr_model = registry.get_model(ModelType.LR)
    cloned_lr_model.description = "Linear Regression Model Cloned"
    cloned_svm_model = registry.get_model(ModelType.SVM)
    cloned_svm_model.description = "Support Vector Machine Model Cloned"
    print(lr_model)
    print(cloned_lr_model)
    print(svm_model)
    print(cloned_svm_model)

```

### Prototype Pattern in Golang

```go
package prototype

type ObjectClonable interface {
	Clone() *MLModel
}

type MLModel struct {
	modelType       ModelType
	description     string
	trainingSplit   float64
	validationSplit float64
	alpha           float64
	beta            float64
}

func (m *MLModel) Clone() *MLModel {
	return &MLModel{
		modelType:       m.modelType,
		description:     m.description,
		trainingSplit:   m.trainingSplit,
		validationSplit: m.validationSplit,
		alpha:           m.alpha,
		beta:            m.beta,
	}
}

// Getter and Setter methods
func (m *MLModel) GetModelType() ModelType {
	return m.modelType
}

func (m *MLModel) SetModelType(modelType ModelType) {
	m.modelType = modelType
}

func (m *MLModel) GetDescription() string {
	return m.description
}

func (m *MLModel) SetDescription(description string) {
	m.description = description
}

func (m *MLModel) GetTrainingSplit() float64 {
	return m.trainingSplit
}

func (m *MLModel) SetTrainingSplit(trainingSplit float64) {
	m.trainingSplit = trainingSplit
}

func (m *MLModel) GetValidationSplit() float64 {
	return m.validationSplit
}

func (m *MLModel) SetValidationSplit(validationSplit float64) {
	m.validationSplit = validationSplit
}

func (m *MLModel) GetAlpha() float64 {
	return m.alpha
}

func (m *MLModel) SetAlpha(alpha float64) {
	m.alpha = alpha
}

func (m *MLModel) GetBeta() float64 {
	return m.beta
}

func (m *MLModel) SetBeta(beta float64) {
	m.beta = beta
}

type ModelType string

const (
	LR  ModelType = "LR"
	SVM ModelType = "SVM"
	DT  ModelType = "DT"
)

type ModelRegistry struct {
	models map[ModelType]*MLModel
}

func NewModelRegistry() *ModelRegistry {
	return &ModelRegistry{
		models: make(map[ModelType]*MLModel),
	}
}

func (r *ModelRegistry) RegisterModel(model *MLModel) {
	r.models[model.GetModelType()] = model
}

func (r *ModelRegistry) GetModel(modelType ModelType) *MLModel {
	if model, exists := r.models[modelType]; exists {
		return model.Clone()
	}
	return nil
}
```
---
## Factory
- is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created. 
- This is done by creating objects by calling a factory method—either specified in an interface and implemented by child classes, or implemented in a base class and optionally overridden by derived classes—rather than by calling a constructor.
- The client code can request an object from a factory object without having to know the class of the object that will be returned. The factory object can create the object and return it to the client code.

### Simple Factory
- is a creational pattern that provides a static method for creating objects.
- The method can be used to create objects without having to specify the exact class of the object that will be created. This is done by creating a factory class that contains a static method for creating objects.

```java
class UserFactory {
	public static User createUser(UserRole role) {
		switch (role) {
			case STUDENT:
				return new Student("John", "Doe");
			case TEACHER:
				return new Teacher("John", "Doe");
			case ADMIN:
				return new Admin("John", "Doe");
		}
	}
}

...

User user = UserFactory.createUser(UserRole.STUDENT);
```

- The complete steps to implement the simple factory pattern are:
	- **Factory class** - Create a factory class that contains a static method for creating objects
	- **Conditional** - Use a conditional statement to create the object based on the input.
	- **Request** - Request an object from the factory class without having to know the class of the object that will be returned
	
### Factory Method
- simple factory method is easy to implement, but it has a few drawbacks. 
- The factory class is not extensible. If we want to add a new type of user, we will have to modify the factory class. 
- Also, the factory class is not reusable. If we want to create a factory for creating different types of objects, we will have to create a new factory class. 
- To overcome these drawbacks, we can use the factory method pattern.

- In the factory method the responsibility of creating the object is shifted to the child classes. 
- The factory method is implemented in the base class and the child classes can override the factory method to create objects of their own type. 
- The factory method is also known as the virtual constructor.

```java 
@AllArgsContructor
abstract class UserFactory {
	public abstract User createUser(String firstName, String lastName);
}

class StudentFactory extends UserFactory {
	@Override
	public User createUser(String firstName, String lastName) {
		return new Student(firstName, lastName);
	}
}

...

UserFactory factory = new StudentFactory();
User user = factory.createUser("John", "Doe");
```

- The complete steps to implement the factory method pattern are:
	- **Base factory interface** - Create a factory class that contains a method for creating objects.
	- **Child factory class** - Create a child class that extends the base factory class and overrides the factory method to create objects of its own type.
	- **Request** - Request an object from the factory class without having to know the class of the object that will be returned.
#### Factory Method in Python

```python
from abc import ABC, abstractmethod

class Button(ABC):
    def __init__(self, border: float):
        self._border = border

    @property
    def border(self) -> float:
        return self._border

    @border.setter
    def border(self, value: float):
        self._border = value

    @abstractmethod
    def render(self):
        pass

    @abstractmethod
    def onClick(self):
        pass

class ButtonFactory(ABC):
    @abstractmethod
    def createButton(self, border: float, radius: float, length: float) -> Button:
        pass

class RoundButton(Button):
    def __init__(self, border: float, radius: float):
        super().__init__(border)
        self._radius = radius

    @property
    def radius(self) -> float:
        return self._radius

    def onClick(self):
        print("Round Button was clicked!")

    def render(self):
        print("Rendered!")

class RoundButtonFactory(ButtonFactory):
    def createButton(self, border: float, radius: float, length: float) -> Button:
        return RoundButton(border, radius)
    
class SquareButton(Button):
    def __init__(self, border: float, length: float):
        super().__init__(border)
        self._length = length

    @property
    def length(self) -> float:
        return self._length

    def onClick(self):
        print("Square Button was clicked!")

    def render(self):
        print("Rendered!")

class SquareButtonFactory(ButtonFactory):
    def createButton(self, border: float, radius: float, length: float) -> Button:
        return SquareButton(border, length)
    
def client_code(factory: ButtonFactory):
    button = factory.createButton(border=1.0, radius=5.0, length=10.0)

    button.render()
    button.onClick()

def main():
    round_button_factory = RoundButtonFactory()
    print("Using RoundButtonFactory:")
    client_code(round_button_factory)

    square_button_factory = SquareButtonFactory()
    print("\nUsing SquareButtonFactory:")
    client_code(square_button_factory)

if __name__ == "__main__":
    main()
```

#### Factory Method in Golang

```go
package factorymethod

import "fmt"

// Button is the abstract product
type Button interface {
	Render()
	OnClick()
	GetBorder() float64
	SetBorder(border float64)
}

// RoundButton is a concrete product
type RoundButton struct {
	border float64
	radius float64
}

func (b *RoundButton) Render() {
	fmt.Println("Rendered RoundButton!")
}

func (b *RoundButton) OnClick() {
	fmt.Println("Round Button was clicked!")
}

func (b *RoundButton) GetBorder() float64 {
	return b.border
}

func (b *RoundButton) SetBorder(border float64) {
	b.border = border
}

func (b *RoundButton) GetRadius() float64 {
	return b.radius
}

func (b *RoundButton) SetRadius(radius float64) {
	b.radius = radius
}

// SquareButton is a concrete product
type SquareButton struct {
	border float64
	length float64
}

func (b *SquareButton) Render() {
	fmt.Println("Rendered SquareButton!")
}

func (b *SquareButton) OnClick() {
	fmt.Println("Square Button was clicked!")
}

func (b *SquareButton) GetBorder() float64 {
	return b.border
}

func (b *SquareButton) SetBorder(border float64) {
	b.border = border
}

func (b *SquareButton) GetLength() float64 {
	return b.length
}

func (b *SquareButton) SetLength(length float64) {
	b.length = length
}

// ButtonFactory is the abstract factory
type ButtonFactory interface {
	CreateButton(border float64, radius float64, length float64) Button
}

// RoundButtonFactory is a concrete factory
type RoundButtonFactory struct{}

func (f *RoundButtonFactory) CreateButton(border float64, radius float64, length float64) Button {
	return &RoundButton{
		border: border,
		radius: radius,
	}
}

// SquareButtonFactory is a concrete factory
type SquareButtonFactory struct{}

func (f *SquareButtonFactory) CreateButton(border float64, radius float64, length float64) Button {
	return &SquareButton{
		border: border,
		length: length,
	}
}

func ClientCode(factory ButtonFactory) {
	button := factory.CreateButton(1.0, 5.0, 10.0)
	button.Render()
	button.OnClick()
}
```
### Abstract Factory 
- is a creational pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes

**Example**
- We have already created a `User` abstract class. Now we will create the concrete classes `Student` and `Teacher`. 
- To restrict the usage of subclasses, we can create factories for each of the concrete classes. The `StudentFactory` will be used to create `Student` objects and the `TeacherFactory` will be used to create `Teacher` objects.

```java 
class StudentFactory {
	public User createStudent(String firstName, String lastName) {
		return new Student(firstName, lastName);
	}
}

class TeacherFactory {
	public User createTeacher(String firstName, String lastName) {
		return new Teacher(firstName, lastName);
	}
}

...

StudentFactory studentFactory = new StudentFactory();
Student student = studentFactory.createStudent("John", "Doe");

TeacherFactory teacherFactory = new TeacherFactory();
Teacher teacher = teacherFactory.createTeacher("John", "Doe");
```

- But now we have a problem, we can use the factories to create any type of student and teacher. Should a teacher teaching Physics be able to teach a student of Biology class? 
- This is where the concept of related or a family of objects comes into play. The `Student` and `Teacher` objects are related to each other. 
- A teacher should only be able to teach a student of the same class. So we can create a factory that can create a family of related objects. The `ClassroomFactory` will be used to create `Student` and `Teacher` objects of the same class.

```java 
abstract class ClassroomFactory {
	public abstract Student createStudent(String firstName, String lastName);
	public abstract Teacher createTeacher(String firstName, String lastName);
}
```

- Now we can create concrete factories for each family of related objects that we want to create.

```java 
class BiologyClassroomFactory extends ClassroomFactory {
	@Override
	public Student createStudent(String firstName, String lastName) {
		return new BiologyStudent(firstName, lastName);
	}
 
	@Override
	public Teacher createTeacher(String firstName, String lastName) {
		return new BiologyTeacher(firstName, lastName);
	}
}
```

- The class `ClassroomFactory` is an abstract class that contains the factory methods for creating the objects. 
- The child classes can override the factory methods to create objects of their own type. The client code can request an object from the factory class without having to know the class of the object that will be returned.

```java 
ClassroomFactory factory = new BiologyClassroomFactory();
Student student = factory.createStudent("John", "Doe");
Teacher teacher = factory.createTeacher("John", "Doe");
```

- The class `ClassroomFactory` becomes our abstract factory that essentially is a factory of factories.

#### Abstract Factory in Python

```python
from abc import ABC, abstractmethod

# Abstract Product: Button
class Button(ABC):
    def __init__(self, border: float):
        self._border = border

    @property
    def border(self) -> float:
        return self._border

    @abstractmethod
    def render(self):
        pass

    @abstractmethod
    def onClick(self):
        pass

# Concrete Product: DarkButton
class DarkButton(Button):
    def __init__(self, border: float, radius: float):
        super().__init__(border)
        self._radius = radius

    @property
    def radius(self) -> float:
        return self._radius

    def onClick(self):
        print("Dark Btn was clicked!")

    def render(self):
        print("Rendered!")

# Abstract Product: Radio
class Radio(ABC):
    @abstractmethod
    def onSelect(self):
        pass

    @abstractmethod
    def render(self):
        pass

# Concrete Product: DarkRadio
class DarkRadio(Radio):
    def onSelect(self):
        print("DarkRadio selected!")

    def render(self):
        print("DarkRadio rendered!")

# Abstract Factory
class ThemeFactory(ABC):
    @abstractmethod
    def createButton(self, border: float, length: float, radius: float) -> Button:
        pass

    @abstractmethod
    def createRadio(self) -> Radio:
        pass

# Concrete Factory: DarkThemeFactory
class DarkThemeFactory(ThemeFactory):
    def createButton(self, border: float, length: float, radius: float) -> Button:
        return DarkButton(border, radius)

    def createRadio(self) -> Radio:
        return DarkRadio()

# Concrete Product: LightButton
class LightButton(Button):
    def __init__(self, border: float, length: float):
        super().__init__(border)
        self._length = length

    @property
    def length(self) -> float:
        return self._length

    def onClick(self):
        print("Light Btn was clicked!")

    def render(self):
        print("Rendered!")

# Concrete Product: LightRadio
class LightRadio(Radio):
    def onSelect(self):
        print("LightRadio selected!")

    def render(self):
        print("LightRadio rendered!")

# Concrete Factory: LightThemeFactory
class LightThemeFactory(ThemeFactory):
    def createButton(self, border: float, length: float, radius: float) -> Button:
        return LightButton(border, length)

    def createRadio(self) -> Radio:
        return LightRadio()

# Client code
def client_code(factory: ThemeFactory):
    button = factory.createButton(border=1.0, length=10.0, radius=5.0)
    button.render()
    button.onClick()

    radio = factory.createRadio()
    radio.render()
    radio.onSelect()

def main():
    print("Using DarkThemeFactory:")
    dark_factory = DarkThemeFactory()
    client_code(dark_factory)

    print("\nUsing LightThemeFactory:")
    light_factory = LightThemeFactory()
    client_code(light_factory)

if __name__ == "__main__":
    main()
```

#### Abstract Factory in Golang

```go
package abstractfactory

import "fmt"

// Button is the abstract product
type Button interface {
	Render()
	OnClick()
	GetBorder() float64
}

// DarkButton is a concrete product
type DarkButton struct {
	border float64
	radius float64
}

func (b *DarkButton) Render() {
	fmt.Println("Rendered DarkButton!")
}

func (b *DarkButton) OnClick() {
	fmt.Println("Dark Btn was clicked!")
}

func (b *DarkButton) GetBorder() float64 {
	return b.border
}

func (b *DarkButton) GetRadius() float64 {
	return b.radius
}

// Radio is the abstract product
type Radio interface {
	OnSelect()
	Render()
}

// DarkRadio is a concrete product
type DarkRadio struct{}

func (r *DarkRadio) OnSelect() {
	fmt.Println("DarkRadio selected!")
}

func (r *DarkRadio) Render() {
	fmt.Println("DarkRadio rendered!")
}

// LightButton is a concrete product
type LightButton struct {
	border float64
	length float64
}

func (b *LightButton) Render() {
	fmt.Println("Rendered LightButton!")
}

func (b *LightButton) OnClick() {
	fmt.Println("Light Btn was clicked!")
}

func (b *LightButton) GetBorder() float64 {
	return b.border
}

func (b *LightButton) GetLength() float64 {
	return b.length
}

// LightRadio is a concrete product
type LightRadio struct{}

func (r *LightRadio) OnSelect() {
	fmt.Println("LightRadio selected!")
}

func (r *LightRadio) Render() {
	fmt.Println("LightRadio rendered!")
}

// ThemeFactory is the abstract factory
type ThemeFactory interface {
	CreateButton(border, length, radius float64) Button
	CreateRadio() Radio
}

// DarkThemeFactory is a concrete factory
type DarkThemeFactory struct{}

func (f *DarkThemeFactory) CreateButton(border, length, radius float64) Button {
	return &DarkButton{border: border, radius: radius}
}

func (f *DarkThemeFactory) CreateRadio() Radio {
	return &DarkRadio{}
}

// LightThemeFactory is a concrete factory
type LightThemeFactory struct{}

func (f *LightThemeFactory) CreateButton(border, length, radius float64) Button {
	return &LightButton{border: border, length: length}
}

func (f *LightThemeFactory) CreateRadio() Radio {
	return &LightRadio{}
}

// Client code
func ClientCode(factory ThemeFactory) {
	button := factory.CreateButton(1.0, 10.0, 5.0)
	button.Render()
	button.OnClick()

	radio := factory.CreateRadio()
	radio.Render()
	radio.OnSelect()
}
```

#### Advantages of Abstract Factory
- **Isolate concrete classes** - The client code is not coupled to the concrete classes of the objects that it creates.
- **Easy to exchange product families** - The client code can request an object from the factory class without having to know the class of the object that will be returned. This makes it easy to exchange product families.
- **Promotes consistency among products** - The client code can request an object from the factory class without having to know the class of the object that will be returned. This makes it easy to maintain consistency among products.
---
**References:**
[Github](https://github.com/abhiabhi0/design-patterns)