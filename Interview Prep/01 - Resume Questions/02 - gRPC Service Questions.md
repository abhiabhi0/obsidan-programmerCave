**1. Why did you choose gRPC for handling user offer data?**

- **Answer:**
    - gRPC provides high-performance communication with low latency due to its binary serialization format (Protocol Buffers).
    - It supports bidirectional streaming, which is useful for real-time data updates.
    - Strongly-typed contracts in `.proto` files make APIs easy to maintain and less error-prone.

---

**2. Can you explain the structure of the gRPC service you developed?**
- I worked on Bulk subscription migration, migrating multiple users from current offer to target offer
- Upload csv file with user ids and their current offer and another value with target offer.
---

**3. How did you handle authentication and security in the gRPC service?**

- **Answer:**
    - Added token-based authentication using metadata in gRPC requests (e.g., JWT). [[JSON Web Token (JWT)]]
    - Ensured role-based access control to prevent unauthorized operations.

---

**4. What challenges did you face while implementing the gRPC service?**

[[13 - Challenges Faced While Implementing the gRPC Service]]

---

**5. How did you ensure reliability and scalability of the gRPC service?**

- **Answer:**
    - Deployed the service with Kubernetes, using horizontal pod autoscaling based on CPU/memory usage.
    - Added retries with exponential backoff for transient failures.
    - Used load balancing to distribute traffic evenly across service instances.

---

#### **Integration Questions**

**12. How did the gRPC service and Kafka integration work together?**

- **Answer:**
    - The gRPC service acted as a producer, sending user offer data as messages to Kafka.
    - The Kafka consumer subscribed to the topic and processed these messages to update user offer codes dynamically.

---

**13. How did you ensure data consistency between the gRPC service, Kafka, and the database?**

- **Answer:**
    - Used transactional producers to ensure messages were published atomically with database updates.
    - Designed the consumer to handle duplicate messages idempotently, ensuring no data corruption.
    - Monitored lag to ensure timely processing of Kafka messages.

---

**14. How did you monitor the entire pipeline (gRPC service, Kafka producer, and consumer)?**

- **Answer:**
    - Added metrics using Prometheus to track producer and consumer performance (e.g., message rates, errors, latencies).
    - Used Datadog dashboards to monitor Kafka topics, consumer lag, and system health.
    - Configured alerts for anomalies like high message lag or failed retries.

---

Let me know if you want deeper explanations, more technical details, or example scenarios!