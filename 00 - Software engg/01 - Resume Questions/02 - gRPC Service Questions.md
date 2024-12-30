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