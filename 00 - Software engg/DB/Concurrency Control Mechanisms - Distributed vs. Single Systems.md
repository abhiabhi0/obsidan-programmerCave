1. **Pessimistic Locking:**
    - **Definition:**
        - Locks resources preemptively to prevent conflicts.
        - Risks deadlocks, which occur when transactions wait for each other’s locks indefinitely.
        - Handled using timeouts and transaction rollbacks.
    
    - **Distributed Systems:**    
        - Used in systems where consistency is critical, like distributed databases (e.g., MySQL Cluster).
        - Requires a distributed locking mechanism such as **Zookeeper** or **Redlock** (Redis-based). [[Mitigate race condition in Distributed System]]
        - Higher latency due to network communication between nodes.
            
    - **Single Systems:**
        - Easier to implement as the lock management is centralized.
        - Ideal for transactional systems with heavy contention on shared resources.
            
2. **Optimistic Locking:**
    - **Definition:**
        - Assumes conflicts are rare and checks for them only during updates.
        - Uses version numbers or timestamps to detect modifications.
        - Eliminates deadlocks but requires retry mechanisms for concurrent updates.
            
    - **Distributed Systems:**
        - Common in microservices architectures where each service operates independently.
        - Suitable for event-driven systems where retries are acceptable.
        - Examples: Inventory management in e-commerce platforms.
            
    - **Single Systems:**
        - Used in low-contention scenarios within standalone databases.
        - Avoids overhead associated with traditional locks, improving performance for updates.
            
3. **Multi-Version Concurrency Control (MVCC):**
    
    - **Definition:**
        - Maintains multiple versions of a row.
        - Reads return consistent snapshots of data, even during updates.
        - Writes are rolled back if a conflict with a newer version is detected.
            
    - **Distributed Systems:**
        - Frequently employed in NoSQL databases like **Cassandra** and **CockroachDB** or RDBMS like **PostgreSQL**.
        - Ensures consistent reads across nodes by providing snapshots of data at a specific version.
        - Enables high availability by allowing reads without blocking writes.
            
    - **Single Systems:**
        - Used in RDBMS like PostgreSQL for providing consistent reads in analytical queries.
        - Simplifies handling concurrent transactions without introducing significant locking overhead.
[[Database Locks - Key Concepts and Types]]
