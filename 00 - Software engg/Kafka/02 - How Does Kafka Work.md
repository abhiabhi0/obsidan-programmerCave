#### 1. **Producers Send Messages**:

- Producers are applications or systems that send **messages** (data) to Kafka topics.
- Kafka determines **which partition** a message goes to using:
    - **Round-Robin**: Messages are evenly distributed across all partitions.
    - **Key-based Partitioning**: If a producer specifies a key (e.g., `user_id`), Kafka uses a hash function to send all messages with the same key to the same partition.
    - **Custom Partitioners**: Producers can implement their own partitioning logic.

---

#### 2. **Partitions Store the Data**:

- Each partition is stored on one or more brokers in the Kafka cluster.
- For **fault tolerance**, partitions are replicated across brokers. This means:
    - One broker stores the **leader** replica of the partition (handles reads/writes).
    - Other brokers store **follower** replicas (for redundancy).
- Example:
    - Topic `orders` with 3 partitions:
        - Partition 0: Stored on Broker 1 (leader), replicated to Broker 2 and Broker 3.
        - Partition 1: Stored on Broker 2 (leader), replicated to Broker 1 and Broker 3.
        - Partition 2: Stored on Broker 3 (leader), replicated to Broker 1 and Broker 2.

---

#### 3. **Consumers Read Messages**:

- Consumers read data from partitions in the order messages were stored (offset order).
- Kafka supports **Consumer Groups** for load balancing:
    - Each partition is consumed by only one consumer within a group.
    - Example:
        - 3 partitions in the `orders` topic.
        - Consumer Group A with 3 consumers (C1, C2, C3):
            - Partition 0 → C1
            - Partition 1 → C2
            - Partition 2 → C3