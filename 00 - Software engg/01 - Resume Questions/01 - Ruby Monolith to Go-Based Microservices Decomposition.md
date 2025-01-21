#### **1. Why did you decide to decompose the Ruby monolith into microservices?**

- **Answer:**  
    The monolith posed challenges like slower deployment cycles, scalability bottlenecks, and difficulty in isolating faults. Decomposing it into Go-based microservices allowed us to achieve better scalability, independent deployments, and clearer ownership of services by respective teams.

#### **2. How did you approach the decomposition process?**

- **Answer:**
    - Prioritized high-impact areas to extract first.
    - Collaborated with stakeholders to define APIs and interactions between services.
    - Ensured robust observability by implementing logs, metrics, and traces during migration.
    - Used a strangler pattern for incremental migration while maintaining existing functionality.

#### **3. Why did you choose Go for the microservices?**

- **Answer:**
    - Go’s lightweight runtime and simplicity suited our needs for fast, efficient, and scalable services.
    - It provides excellent support for concurrency, making it ideal for services handling high-throughput tasks.
    - Go’s static typing and single binary deployment reduced runtime errors and simplified the CI/CD pipeline. [[0106 - Go’s Static Typing and Single Binary Deployment]]

#### **4. What challenges did you face during the decomposition?**

- **Answer:**
    - Aligning teams on new service boundaries and data ownership.
    - Handling data migration while ensuring consistency across services.
    - Maintaining feature parity and avoiding downtime during the transition.
    - Testing and debugging distributed systems, including ensuring fault tolerance.
    - Overcoming cultural resistance to adopting new technologies like Go.

#### **5. How did you ensure smooth communication between the new microservices?**

- **Answer:**
    - Designed RESTful APIs with clear documentation for service interaction.
    - Leveraged gRPC for high-performance communication where latency was critical.
    - Introduced a service mesh for observability, retries, and load balancing.
    - Implemented consistent API versioning to support backward compatibility.

#### **6. How did you handle data consistency across the microservices?**

- **Answer:**
    - Adopted eventual consistency using message queues for asynchronous communication.
    - Implemented distributed transactions using SAGA patterns for critical workflows.
    - Utilized caching layers to reduce frequent database lookups across services.

#### **7. Can you describe a specific service you worked on during the decomposition?**
- I was working in plus team, so I have made changes from Plus team's perspective in other teams repo.
- We wanted to show different plus upsell based on user's subscription status, the current amount of basket etc..

#### **8. How did you ensure collaboration across cross-functional teams?**

- **Answer:**
    - Conducted regular meetings to align on goals and progress.
    - Shared detailed documentation and API contracts.
    - Set up Slack channels and dashboards for real-time communication and tracking issues.
    - Organized workshops to train teams on Go and microservices best practices.