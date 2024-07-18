**Decorator**
 - structural design pattern that allows adding new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.

**Facade**
* structural design pattern that provides a simplified interface to a library, a framework, or any other complex set of classes.
---
## Decorator
* attaches additional responsibilities to an object dynamically.
### Problem
- Let us say we want to build a class that sends our users emails with a greeting. We can start with a simple class:

```java
public class EmailService {
    public void sendEmail(String email, String message) {
        ...
    }
}
```

* As the application grows we may want to add some additional functionality to our email service. 
* For example, we may want to send phone notifications to our users or send them slack messages. 
* We can obviously add new methods to our EmailService class, but this will violate the Single Responsibility Principle. So we do as we always do and abstract the functionality into a separate class:

```java
public interface Communicator {
    void send(String target, String message);
}
```

* Now we have the following hierarchy:

```mermaid
classDiagram
    Communicator <|-- EmailService
    Communicator <|-- PhoneService
    Communicator <|-- SlackService
    class Communicator {
        +send(String target, String message)
    }

    class EmailService {
        +send(String email, String message)
    }

    class PhoneService {
        +send(String phone, String message)
    }

    class SlackService {
        +send(String slackId, String message)
    }
```

* above hierarchy is fine, but what if we want to send emails to our users and also send them a phone notification? 
* We can use the separate classes, but this will violate the Open-Closed Principle. We can't extend the functionality of our EmailService class, because it is final. So we need to create a new class that will encapsulate the functionality of the EmailService and the PhoneService classes:

```mermaid
classDiagram
    Communicator <|-- EmailService
    Communicator <|-- PhoneService
    Communicator <|-- SlackService
    Communicator <|-- EmailPhoneService
    class Communicator {
        +send(String target, String message)
    }

    class EmailService {
        +send(String email, String message)
    }

    class PhoneService {
        +send(String phone, String message)
    }

    class SlackService {
        +send(String slackId, String message)
    }

    class EmailPhoneService {
        +send(String target, String message)
    }
```

* The above approach is extremely brittle. If we want to add a new functionality, we will have to create a new class for each combination of existing functionalities. It also leads to class explosion. 
* The number of classes grows exponentially with the number of possible combinations of functionalities.

### Solution (Java)
* The problem with the above approach is that inheritance is static. We can't add new functionality to an existing class at runtime.

* Another option is to use composition. 
* We can create a new class that will contain references to the existing classes and delegate the calls to the existing classes. This where the Decorator pattern comes in. 
* The **Decorator pattern** allows us to add new functionality to an existing object without altering its structure. The Decorator pattern is also known as **Wrapper**.

* A **wrapper** is an object that can be linked with some target object. The wrapper contains the same set of methods as the target and delegates to it all requests it receives. 
* However, the wrapper may alter the result by doing something either before or after it passes the request to the target. The wrapper implements the same interface as the wrapped object. 

* The Decorator pattern is implemented with the following steps:
  - **Common product interface** - Declare the common interface for both wrappers and wrapped objects.
```java
public interface Communicator {
    void send(String target, String message);
}
```

 - **Concrete product** - Create a concrete product class that implements the common interface and represents the basic behavior of the wrapped object.
```java
public class EmailService implements Communicator {
    @Override
    public void send(String email, String message) {
        ...
    }
}
```

- **Base decorator** - Create a base decorator class that implements the common interface and contains a field for storing a reference to a wrapped object.
```java
public abstract class CommunicatorDecorator implements Communicator {
    protected Communicator communicator;

    public CommunicatorDecorator(Communicator communicator) {
        this.communicator = communicator;
    }
}
```

- **Concrete decorators** - Create concrete decorator classes that extend the base decorator class and add additional behavior to the wrapped object.
```java
public class PhoneService extends CommunicatorDecorator {
    public PhoneService(Communicator communicator) {
        super(communicator);
    }

    @Override
    public void send(String phone, String message) {
        communicator.send(phone, message);
        sendPhoneNotification(phone, message);
    }

    private void sendPhoneNotification(String phone, String message) {
        ...
    }
}
```

- **Client** - The client code works with all objects using the common interface. This way it can stay independent of the concrete classes of objects it works with.

```java
public class Client {
    public static void main(String[] args) {
        Communicator communicator = new EmailService();
        Communicator phoneService = new PhoneService(communicator);
        Communicator slackService = new SlackService(phoneService);
        slackService.send("user", "Hello");
    }
}
```

### Decorator Pattern in Python

```python
from abc import ABC, abstractmethod

class Datasource(ABC):
    @abstractmethod
    def read(self):
        pass

    @abstractmethod
    def write(self, value):
        pass

class BaseDecorator(Datasource):
    def __init__(self, next_layer):
        self._next_layer = next_layer

class FileDatasource(Datasource):
    def read(self):
        return "Base"

    def write(self, value):
        print(value)

class CompressionDecorator(BaseDecorator):
    def __init__(self, datasource):
        super().__init__(datasource)

    def read(self):
        compressed = self._next_layer.read()
        return self._decompress(compressed)

    def _decompress(self, compressed):
        return f"{compressed} - Decompressed"

    def write(self, value):
        compressed = self._compress(value)
        self._next_layer.write(compressed)

    def _compress(self, value):
        return f"{value} - Compressed"

class EncryptionDecorator(BaseDecorator):
    def __init__(self, next_layer):
        super().__init__(next_layer)

    def read(self):
        value = self._next_layer.read()
        return self._decrypt(value)

    def _decrypt(self, value):
        return f"{value} - Decrypted"

    def write(self, value):
        encrypted = self._encrypt(value)
        self._next_layer.write(encrypted)

    def _encrypt(self, value):
        return f"{value} - Encrypted"

if __name__ == "__main__":
    datasource = FileDatasource()
    encrypted_datasource = EncryptionDecorator(datasource)
    compressed_encrypted_datasource = CompressionDecorator(encrypted_datasource)

    compressed_encrypted_datasource.write("Test Data")
    print(compressed_encrypted_datasource.read())
```

### Decorator Pattern in Golang

```go
package decorator

import "fmt"

type Datasource interface {
	Read() string
	Write(value string)
}

type BaseDecorator struct {
	nextLayer Datasource
}

func (b *BaseDecorator) Read() string {
	return b.nextLayer.Read()
}

func (b *BaseDecorator) Write(value string) {
	b.nextLayer.Write(value)
}

type FileDatasource struct{}

func (f *FileDatasource) Read() string {
	return "Base"
}

func (f *FileDatasource) Write(value string) {
	fmt.Println(value)
}

type CompressionDecorator struct {
	BaseDecorator
}

func NewCompressionDecorator(datasource Datasource) *CompressionDecorator {
	return &CompressionDecorator{
		BaseDecorator{nextLayer: datasource},
	}
}

func (c *CompressionDecorator) Read() string {
	compressed := c.nextLayer.Read()
	return c.decompress(compressed)
}

func (c *CompressionDecorator) decompress(compressed string) string {
	return compressed + " - Decompressed"
}

func (c *CompressionDecorator) Write(value string) {
	compressed := c.compress(value)
	c.nextLayer.Write(compressed)
}

func (c *CompressionDecorator) compress(value string) string {
	return value + " - Compressed"
}

type EncryptionDecorator struct {
	BaseDecorator
}

func NewEncryptionDecorator(datasource Datasource) *EncryptionDecorator {
	return &EncryptionDecorator{
		BaseDecorator{nextLayer: datasource},
	}
}

func (e *EncryptionDecorator) Read() string {
	value := e.nextLayer.Read()
	return e.decrypt(value)
}

func (e *EncryptionDecorator) decrypt(value string) string {
	return value + " - Decrypted"
}

func (e *EncryptionDecorator) Write(value string) {
	encrypted := e.encrypt(value)
	e.nextLayer.Write(encrypted)
}

func (e *EncryptionDecorator) encrypt(value string) string {
	return value + " - Encrypted"
}
```
### Advantages
* Object behavior can be extended at runtime by wrapping an object with one or several decorators without creating a new subclass.
* Runtime configuration of an object is possible.
* New behavior can be added to an object without changing its code.
* SRP is respected by encapsulating the behavior in a separate class.
---
## Facade
* structural design pattern that provides a simplified interface to a library, a framework, or any other complex set of classes.
### Problem
* Let us take the example of an e-commerce application. 
* The application has a lot of functionality. It has a product catalog, a shopping cart, a payment system, a shipping system, etc. The application has a lot of classes and a lot of dependencies between them. The application is complex and it is hard to understand how all the classes work together. 
* When you make an order, you have to do the following:
    * Call payment gateway to charge the credit card.
    * Update the inventory.
    * Email the customer.
    * Add the order to the shipping queue.
    * Update analytics.

```java
public class Order {
    private PaymentGateway paymentGateway;
    private Inventory inventory;
    private EmailService emailService;
    private ShippingService shippingService;
    private AnalyticsService analyticsService;

    public void checkout() {
        paymentGateway.charge();
        inventory.update();
        emailService.send();
        shippingService.add();
        analyticsService.update();
    }
}
```

* Here we have a lot of dependencies, some of which might be external vendors. 
* The business logic of your classes would become tightly coupled to the implementation details of 3rd-party classes, making it hard to comprehend and maintain. 
* The Order class is hard to test. You will have to mock all the dependencies. The Order class is also hard to reuse. If you want to reuse the Order class in another application, you will have to change the code. 
* Every time one of the logic changes, you will have to change the code in multiple places and hence violating SOLID principles.

* A **facade** is a class that provides a simple interface to a complex subsystem which contains lots of moving parts. 
* A facade might provide limited functionality in comparison to working with the subsystem directly. However, it includes only those features that clients really care about.
### Solution (Java)
* Facade pattern suggests that you wrap a complex subsystem with a simpler interface. 
* The Facade pattern provides a higher-level interface that makes the subsystem easier to use. 
* The Facade pattern is implemented by simply creating a new class that encapsulates the complex logic of the existing classes. For our example above, we will move the complex logic to a new class called `OrderProcessor`.

```java
public class OrderProcessor {
    private PaymentGateway paymentGateway;
    private Inventory inventory;
    private EmailService emailService;
    private ShippingService shippingService;
    private AnalyticsService analyticsService;

    public void process() {
        paymentGateway.charge();
        inventory.update();
        emailService.send();
        shippingService.add();
        analyticsService.update();
    }
}
```

* Now we can use the `OrderProcessor` class in our `Order` class and delegate the complex logic to the `OrderProcessor` class.

```java
public class Order {
    private OrderProcessor orderProcessor;

    public void checkout() {
        orderProcessor.process();
    }
}
```

* The `Order` class is now much simpler. It has a single responsibility of creating an order. 
* The `Order` class is also easier to test. You can mock the `OrderProcessor` class. 
* The `Order` class is also easier to reuse. You can reuse the `Order` class in another application without changing the code.

### Facade Pattern in Python

```python
from abc import ABC, abstractmethod

class OrderManager(ABC):
    @abstractmethod
    def create_order(self) -> None:
        pass

class OrderManagerImpl(OrderManager):
    def __init__(self, order_processor: 'OrderProcessor'):
        self.order_processor = order_processor

    def create_order(self) -> None:
        self.order_processor.process()

class AnalyticsService:
    def track(self) -> None:
        print("Analytics created")

class InventoryService:
    def check_inventory(self) -> None:
        print("Inventory checked")

class OrderProcessor:
    def __init__(self, recommendation_service: 'RecommendationService', 
                 payment_service: 'PaymentService', 
                 warehouse_processor: 'WarehouseProcessor'):
        self.recommendation_service = recommendation_service
        self.payment_service = payment_service
        self.warehouse_processor = warehouse_processor

    def process(self) -> None:
        self.warehouse_processor.process()
        self.recommendation_service.recommend()
        self.payment_service.pay()

class OrderFlowProcessor:
    def __init__(self):
        self.payment_service = PaymentService()
        self.inventory_service = InventoryService()
        self.recommendation_service = RecommendationService()
        self.analytics_service = AnalyticsService()

    def process(self) -> None:
        self.payment_service.pay()
        # update
        self.inventory_service.check_inventory()
        # analytics
        self.recommendation_service.recommend()
        self.analytics_service.track()

class PaymentService:
    def pay(self) -> None:
        print("Payment done")

class RecommendationService:
    def recommend(self) -> None:
        print("Recommendation created")

class WarehouseProcessor:
    def __init__(self, inventory_service: InventoryService, 
                 analytics_service: AnalyticsService):
        self.inventory_service = inventory_service
        self.analytics_service = analytics_service

    def process(self) -> None:
        self.inventory_service.check_inventory()
        self.analytics_service.track()

if __name__ == "__main__":
    order_flow_processor = OrderFlowProcessor()
    order_flow_processor.process()
```

### Facade Pattern in Golang

```go
package facade

import "fmt"

// OrderManager interface
type OrderManager interface {
	CreateOrder()
}

// OrderManagerImpl struct
type OrderManagerImpl struct {
	orderProcessor *OrderProcessor
}

func (o *OrderManagerImpl) CreateOrder() {
	o.orderProcessor.Process()
}

// OrderProcessor struct
type OrderProcessor struct {
	recommendationService *RecommendationService
	paymentService        *PaymentService
	warehouseProcessor    *WarehouseProcessor
}

func (o *OrderProcessor) Process() {
	o.warehouseProcessor.Process()
	o.recommendationService.Recommend()
	o.paymentService.Pay()
}

// OrderFlowProcessor struct
type OrderFlowProcessor struct {
	paymentService        *PaymentService
	inventoryService      *InventoryService
	recommendationService *RecommendationService
	analyticsService      *AnalyticsService
}

func NewOrderFlowProcessor() *OrderFlowProcessor {
	return &OrderFlowProcessor{
		paymentService:        &PaymentService{},
		inventoryService:      &InventoryService{},
		recommendationService: &RecommendationService{},
		analyticsService:      &AnalyticsService{},
	}
}

func (o *OrderFlowProcessor) Process() {
	o.paymentService.Pay()
	// update
	o.inventoryService.CheckInventory()
	// analytics
	o.recommendationService.Recommend()
	o.analyticsService.Track()
}

// PaymentService struct
type PaymentService struct{}

func (p *PaymentService) Pay() {
	fmt.Println("Payment done")
}

// RecommendationService struct
type RecommendationService struct{}

func (r *RecommendationService) Recommend() {
	fmt.Println("Recommendation created")
}

// WarehouseProcessor struct
type WarehouseProcessor struct {
	inventoryService *InventoryService
	analyticsService *AnalyticsService
}

func (w *WarehouseProcessor) Process() {
	w.inventoryService.CheckInventory()
	w.analyticsService.Track()
}

// AnalyticsService struct
type AnalyticsService struct{}

func (a *AnalyticsService) Track() {
	fmt.Println("Analytics created")
}

// InventoryService struct
type InventoryService struct{}

func (i *InventoryService) CheckInventory() {
	fmt.Println("Inventory checked")
}
```
