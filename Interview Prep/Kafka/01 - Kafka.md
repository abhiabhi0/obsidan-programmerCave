Apache Kafka is a distributed event streaming platform designed for high-throughput, low-latency ingestion of data streams. It is widely used for building real-time data pipelines and streaming applications. Here’s a comprehensive overview of Kafka’s architecture:
### Summary

1. **Core Components**:
    - **Producers**: Publish data to Kafka topics, select partitions via keys.
    - **Consumers**: Read data from topics, organized in consumer groups.
    - **Brokers**: Kafka servers storing data and managing requests.
    - **Topics & Partitions**: Topics are split into partitions for scalability. [[What is Kafka Partition]]
    - **Replication**: Ensures fault tolerance with leaders and followers.
    - **ZooKeeper**: Manages metadata and leader elections (phasing out for metadata quorum).

2. **Consumer Groups**:
    - Kafka uses consumer groups to manage message consumption.
    - Each consumer in a consumer group is assigned one or more partitions, ensuring that each partition is consumed by only one consumer in the group.
    
1. **Offset Management**:
    - Kafka tracks which messages have been read using offsets.
    - Each consumer maintains its own offset for the partitions it consumes.	
#### Key Features
1. **Decoupling of Producers and Consumers**:
    - Kafka enables producers to send messages independently of consumers, allowing scalability and flexibility in microservices architectures.
    
1. **Message Persistence**:
    - Kafka stores messages for a configurable retention period, enabling fault tolerance and allowing reprocessing.
    - [[Where and How Does Kafka Store Messages]]
    
1. **Load Balancing with Consumer Groups**:
    - Consumer groups enable distributed consumption of messages, ensuring efficient load balancing.
    
1. **Scalability**:
    - Kafka achieves scalability through clustering by adding brokers as data volume increases.
    
1. **Message Order Guarantee**:
    - Kafka guarantees message order within a single partition, critical for applications requiring strict sequencing.
    
1. **Distributed System Capabilities**:
    - Kafka operates in distributed environments, supporting high availability and fault tolerance.

[[02 - How Does Kafka Work]]
[[02 - Message Delivery Guarantees in Kafka]]
### Core Components of Kafka

1. **Producers**:
    - Producers are clients that publish (write) data to Kafka topics.
    - Producers send records to a Kafka topic, typically in batches.
    - They can choose which partition within a topic to send records to, often based on a key.

2. **Consumers**:
    - Consumers are clients that subscribe to (read) data from Kafka topics.
    - Consumers can be part of a consumer group, where each consumer in the group reads a unique subset of the partitions.
    - Kafka ensures that each partition is consumed by only one consumer in the group.

3. **Brokers**:
    - Brokers are Kafka servers that store data and serve client requests.
    - Each broker can handle thousands of partitions and millions of records.
    - Brokers communicate with each other to ensure data replication and fault tolerance.

4. **Topics**:
    - Topics are logical channels to which producers write records and from which consumers read records.
    - Topics are divided into partitions to allow for scalability and parallelism.

5. **Partitions**:
    - Each topic is split into multiple partitions, each being an ordered, immutable sequence of records.
    - Partitions allow Kafka to scale horizontally and handle a large number of records efficiently.

6. **Replication**:
    - Kafka replicates partitions across multiple brokers for fault tolerance.
    - Each partition has one leader and multiple followers. The leader handles all read and write requests for the partition, while followers replicate the data.

7. **ZooKeeper**:
    - ZooKeeper is used by Kafka to manage distributed configurations, leader election, and metadata synchronization.
    - Kafka’s reliance on ZooKeeper is being phased out in favor of its own metadata quorum feature in newer versions.

### Key Architectural Concepts

1. **Log-Based Storage**:
    - Kafka stores records in a log format, where each partition is a log.
    - Records are appended to the end of the log and read in sequential order, which makes I/O operations efficient.

2. **Offset Management**:
    - Each record within a partition has a unique offset, which is an identifier that denotes the position of the record.
    - Consumers track offsets to know which records have been processed.

3. **Message Retention**:
    - Kafka allows configuring how long records are retained, either based on time or storage size.
    - Once the retention period expires, records are deleted to free up space.

4. **Data Durability**:
    - Kafka provides durability through replication. A record is considered committed when it is written to the leader and acknowledged by a certain number of followers.

5. **High Throughput and Low Latency**:
    - Kafka’s design emphasizes high throughput and low latency, making it suitable for real-time data streaming applications.
