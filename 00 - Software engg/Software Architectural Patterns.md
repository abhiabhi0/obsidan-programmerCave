![[{83C64CEA-B5C9-46DD-8E6D-5E3D6C0BDC22}.png]]

### What are Software Architectural Patterns?

- Software architectural patterns are reusable solutions to commonly occurring problems in software architecture.
- They provide templates for designing software systems with best practices and proven approaches.

---

### Common Architectural Patterns

#### 1. Layered (N-Tier) Architecture

- **Description**: Organizes the system into layers, each with specific responsibilities (e.g., Presentation, Business Logic, Data Access).
- **Advantages**:
    - Separation of concerns.
    - Easy to maintain and test individual layers.
- **Disadvantages**:
    - Can lead to performance bottlenecks.
    - Tight coupling between layers.
- **Use Case**: Enterprise applications, web apps.

---

#### 2. Client-Server Architecture

- **Description**: Divides the system into clients (requesters) and servers (providers of resources).
- **Advantages**:
    - Centralized control.
    - Easier management of resources.
- **Disadvantages**:
    - Scalability issues.
    - Single point of failure.
- **Use Case**: Web applications, email systems.

---

#### 3. Microservices Architecture

- **Description**: Breaks down the application into small, independent services that communicate over APIs.
- **Advantages**:
    - Scalability and flexibility.
    - Independent deployment.
- **Disadvantages**:
    - Complex to develop and maintain.
    - Requires robust monitoring and communication.
- **Use Case**: Large-scale systems like e-commerce or streaming platforms.

---

#### 4. Event-Driven Architecture

- **Description**: Components communicate by producing and consuming events.
- **Advantages**:
    - High scalability and responsiveness.
    - Loose coupling between components.
- **Disadvantages**:
    - Complex debugging and testing.
    - Dependency on reliable event handling.
- **Use Case**: Real-time applications, IoT systems.

---

#### 5. Model-View-Controller (MVC)

- **Description**: Separates application logic into three components:
    - **Model**: Manages data and business logic.
    - **View**: Handles UI representation.
    - **Controller**: Handles user input and updates Model and View.
- **Advantages**:
    - Clear separation of concerns.
    - Easier to test and maintain.
- **Disadvantages**:
    - Overhead in simple applications.
- **Use Case**: Web and mobile applications.

---

#### 6. Service-Oriented Architecture (SOA)

- **Description**: Builds applications as a collection of services that interact using standard protocols.
- **Advantages**:
    - Reusability of services.
    - Interoperability across platforms.
- **Disadvantages**:
    - Performance overhead due to service communication.
- **Use Case**: Enterprise systems, legacy integrations.
