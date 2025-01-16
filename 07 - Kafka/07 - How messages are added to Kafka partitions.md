## Overview of Kafka Partitions

Kafka topics are divided into multiple partitions, which are ordered, immutable sequences of records. Each partition serves as an independent log where messages are appended in the order they are received. This design allows Kafka to scale horizontally and handle high throughput by distributing data across multiple brokers in a cluster.

## How Messages Are Added to Partitions

### 1. **Producer Configuration**
When a producer sends data to Kafka, it first determines how to assign messages to partitions. The producer can either specify a partition explicitly or rely on a partitioning strategy.

### 2. **Partitioning Strategies**
Kafka provides several strategies for determining which partition a message should go to:

- **Key-Based Partitioning**: 
  - If the producer provides a key with the message, Kafka uses a hashing function on that key to determine the target partition. The formula used is typically:
    $$
    \text{partition} = \text{hash(key)} \mod \text{number of partitions}
    $$
  - This ensures that all messages with the same key are sent to the same partition, preserving their order. This is particularly useful for scenarios where order matters (e.g., processing transactions for a specific user).

- **Round-Robin Partitioning**:
  - If no key is provided, or if the producer opts not to use key-based partitioning, Kafka can distribute messages evenly across all available partitions using a round-robin approach. This method helps balance the load across partitions but does not guarantee message order.

- **Custom Partitioners**:
  - Producers can implement custom partitioning logic by defining their own partitioner class. This allows for more complex routing based on application-specific requirements.

### 3. **Appending Messages**
Once the target partition is determined:
- The producer sends the message to the appropriate broker that hosts the designated partition.
- The broker appends the message to the end of the partition log, assigning it a unique offset. This offset acts as an identifier for each message within that partition and ensures that messages can be retrieved in the order they were sent.

### 4. **Replication and Fault Tolerance**
To ensure durability and availability:
- Each partition can have multiple replicas distributed across different brokers in the cluster. The leader replica handles all reads and writes for that partition, while follower replicas replicate data from the leader.
- This replication mechanism provides fault tolerance; if a broker fails, other replicas can take over with minimal disruption.

### 5. **Message Retention**
Kafka retains messages in partitions for a configurable retention period, allowing consumers to read messages at their own pace without losing data immediately after consumption.

## Conclusion
The process of adding messages to partitions in Kafka involves determining the appropriate partition through various strategies (key-based, round-robin, or custom), appending messages to the selected partition's log while maintaining order based on offsets, and ensuring data durability through replication. By understanding this mechanism, developers can effectively leverage Kafka’s capabilities for scalable and reliable messaging systems.

Citations:
[1] https://www.datacamp.com/tutorial/kafka-partitions
[2] https://factored.ai/power-of-kafka-partitioning/
[3] https://www.redpanda.com/guides/kafka-tutorial-kafka-partition-strategy
[4] https://www.openlogic.com/blog/kafka-partitions
[5] https://stackoverflow.com/questions/38024514/understanding-kafka-topics-and-partitions/51829144
[6] https://newrelic.com/blog/best-practices/effective-strategies-kafka-topic-partitioning
[7] https://www.ibm.com/docs/en/masv-and-l/maximo-manage/continuous-delivery?topic=kafka-message-processing