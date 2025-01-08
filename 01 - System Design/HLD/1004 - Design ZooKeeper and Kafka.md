#### Problem Statement 1: State Tracking in a Master-Slave Architecture
- **Goal**: Ensure clients know the current master and automatically sync when the master changes.
##### Naïve Approach:
- Use a dedicated machine to track the master.
- Issues:
    1. **Single Point of Failure**: If the machine goes down, writes cannot happen.
    2. **Additional Hop**: Every request introduces an extra hop to get master info.
    **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdezP5d7jXALh_7Q1wrEPk804iC9iVtI2pfzHbAAqBdFIjb6Zn86xYPc2Ang4qeQ93vxqcW9E82YpWDbfQRQItGzk9InvLc7P2zbkGzIEocBjYhKPYDISr31tbf8wjLU4FgjnsRmiZwy0EuxeqVgzY9fgc?key=RcMDqZ96s0ud-MH2iUfptA)**
**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcvzEdDAm_S6FojEwDNbcpqrBtK8fqGnJyiWqRhbht6bauJZuh05QJkqdPuKEU_Upgd3lVyXDISWJ9DQbsSrOL0QfXwkc2zdTE4J2YtSfakBMkdUwnzscAi3dke5xNhE2HaT65AP4W-OUMaEIotPzet2ZA?key=RcMDqZ96s0ud-MH2iUfptA)**

##### Improved Solution: ZooKeeper (ZK)
- **ZooKeeper** is a system that maintains strongly consistent data and helps in distributed coordination.
##### ZK Storage Model:
- Structured like a **file system** with directories and files called **ZK Nodes**.
- Types of ZK Nodes:
    1. **Ephemeral Nodes**:
        - Exist only while the session/machine that created them is alive.
        - Require heartbeats to remain valid; otherwise, they are deleted.
        - Example Use Cases: Tracking machine status, master election, distributed locks.
    2. **Persistent Nodes**:
        - Remain until explicitly deleted.
        - Example Use Cases: Storing configuration data.
**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc8biUDhwFxCtNf8fk8iYunCSq29IRkX6Y6GCblRYMzhmWVA4QxJpIcl4P1MlvNPulKHeRQVwRtry7MSS2wb9eKk7kiZveEUAyq3uscs4aaopoitg8MGBRMXO6kP_g1Mkx0wozVLJN2uelg6G0vElHR3ho?key=RcMDqZ96s0ud-MH2iUfptA)**
##### ZK for Master Election:
1. Storage machines try to write their IP address to an **ephemeral node** (e.g., `/clusterx/master_ip`).
    - Only one write will succeed, and the IP address of the new master is stored.
    - The successful machine is now the **master**.
2. Other machines and app servers read the master’s IP address from this node.
**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdWDDOOUU1wp54TP6v9QXgN7kyhW-q6u4YYlTjA5ovGGQckT3km69U8013Lks29JutRlb10BXf1QOEsaa3R5zF9XQFTZIdxxUFKxRrctBeASri6LRaz3FibuAWkinIXnwwhxw8GErq5MzQMuF8mhLXxMq78?key=RcMDqZ96s0ud-MH2iUfptA)****![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdAQVgShKRIX2sqkTfe7VFjTdbv2HyD8AyAid3oPzuosM9PgYri9Lbk96BiPyVgkJ1ownrxpJ9GUqbeyPZBj89bGG-PUtFNoxtyC9oYktiPFuSnxICw6qygbU-PCGhCEyC7DBNUigVxXzBY2U-Nq1EFSsA?key=RcMDqZ96s0ud-MH2iUfptA)**

##### ZK Watches:
- **Watch Mechanism**:
    - Clients subscribe to updates on a node.
    - ZK notifies clients when the node's data changes or is deleted.
    - Reduces redundant queries and offloads work from ZK.

##### ZK Architecture:
1. **Cluster of Machines**:
    - Uses an odd number of machines to prevent split-brain issues.
    - A leader is elected for coordinating writes.
2. **Leader-Based Write Consistency**:
    - Writes go to the leader, which replicates to other machines.
    - Write succeeds if acknowledged by the majority.
3. **Quorum Requirement**:
    - Ensures consistency by requiring a majority for successful writes.
    - Prevents split-brain scenarios (e.g., two conflicting masters).

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcMG3ASC4gaeeijvHBufxi4unh12Kc-eq6jaamjMU6deTBnUkMV9dl42xMbyqJDc40kSFUT9tUrW8sgHiibfdaYlNIieyXrrdiJUNWAWOhL3MHIYUSe_6pxb1C2SiqZ1FUrcnRGYGQBQBQp_locmoo3PXDy?key=RcMDqZ96s0ud-MH2iUfptA)**
##### Master Failure:
1. When the master fails, it stops sending heartbeats to ZK.
2. The ephemeral node `/clusterx/master_ip` is deleted.
3. Subscribers are notified, and slaves compete to become the new master by writing their IP.
4. App servers fetch the new master IP and update their local cache.

---

#### Problem Statement 2: Asynchronous Task Execution

- **Example**: Sending a message on a messaging platform.
    - Actions: Notify the recipient, send an email (if needed), update analytics.
    - Goal: Return success to the sender immediately without blocking for asynchronous tasks.

##### Solution: Persistent Queue with Pub-Sub Model

- **Persistent Queue**:
    - Durable storage of events on disk.
    - Guarantees event retention within a specified period.

##### Pub-Sub Model:

1. **Publish**:
    - Producers publish events (e.g., message sent) to a queue.
2. **Subscribe**:
    - Consumers subscribe to specific topics to process relevant events.

##### Topics:

- Events are categorized into **topics** to avoid processing unrelated events.
- Example:
    - Topic1: Customer purchases (invoice generation system subscribes).
    - Topic2: Messaging (notification system subscribes).

---

#### Kafka: High-Throughput Persistent Queue
##### Key Terminologies:
1. **Producer**: Publishes events to topics.
2. **Consumer**: Consumes events from topics.
3. **Broker**: A Kafka machine that stores published events.
4. **Partition**:
    - Each topic is divided into partitions for better scalability and parallelism.
    - Ensures all messages with the same key go to the same partition, enabling ordering within the partition.
5. **Retention Period**: Configurable time for retaining events.
6. **Consumer Group**:
    - Multiple consumers working in parallel.
    - Each consumer gets exclusive access to specific partitions.
**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdP-zs5Z7-j2ftuo7-mmkvjiWRLTOI4u4RgwfPvwUnAjZDJiSv0bg8UvPKxqNgq4PG2PPfFbjpsoWQogxJtTSpqjcTXnrIfaAdwnOF0sf0zOKTDYLK3pGomn1XouUg-wpppc5Rnke0IXNxGfMQD4hjTuwqa?key=RcMDqZ96s0ud-MH2iUfptA)**

[[01 - Kafka]]
##### Kafka Features:
1. **Scalability**:
    - Partitions allow sharding of topics across brokers.
    - Consumer groups enable parallel processing of events.
2. **Partitioning by Key**:
    - Ensures messages with the same key (e.g., sender ID) are in the same partition.
3. **Replication**:
    - Configurable replication factor for fault tolerance.
    - Primary and replica partitions are distributed across brokers.
4. **Offset Management**:
    - Tracks how much of the topic has been read.
    - Ensures consumers only process new events.
5. **Broker Redirection**:
    - Producers and consumers can connect to any broker, which will internally redirect them to the appropriate partition.

##### Kafka Failure Tolerance:

1. **Partition Replication**:
    - Events are replicated across multiple brokers.
    - If a broker fails, replicas ensure no data loss.
2. **Consumer Rebalancing**:
    - If a consumer in a group fails, Kafka redistributes partitions among remaining consumers.

---

#### Common Challenges and Solutions:

1. **Large Topics**:
    
    - Use partitions to distribute the topic across brokers.
    - Ensure messages with the same key go to the same partition for order preservation.
2. **High Consumer Load**:
    
    - Use consumer groups to parallelize consumption.
    - Avoid having more consumers than partitions.
3. **Message Ordering**:
    
    - Ordering is guaranteed within a partition but not across partitions.
    - Use a consistent key for messages requiring order.
4. **Fault Tolerance**:
    
    - Use replication to prevent data loss.
    - Configure a sufficient replication factor for critical topics.
5. **Efficient Offsets**:
    
    - Track consumer offsets to fetch only new events.

---

#### References

- **ZooKeeper**:
    
    - Design Goals: [https://zookeeper.apache.org/doc/current/zookeeperOver.html#sc_designGoals](https://zookeeper.apache.org/doc/current/zookeeperOver.html#sc_designGoals)
    - Observer Pattern: [ObserverBean.java](https://apache.googlesource.com/zookeeper/+/3d2e0d91fb2f266b32da889da53aa9a0e59e94b2/src/java/main/org/apache/zookeeper/server/ObserverBean.java)
    - Data Model: [https://zookeeper.apache.org/doc/r3.9.1/zookeeperOver.html#sc_dataModelNameSpace](https://zookeeper.apache.org/doc/r3.9.1/zookeeperOver.html#sc_dataModelNameSpace)
- **Kafka**:
    
    - Design: [https://kafka.apache.org/documentation/#design](https://kafka.apache.org/documentation/#design)
    - Consumer Offsets: [https://dattell.com/data-architecture-blog/understanding-kafka-consumer-offset/](https://dattell.com/data-architecture-blog/understanding-kafka-consumer-offset/)
    - Topic and Partitioning: [https://www.conduktor.io/kafka/kafka-topics-choosing-the-replication-factor-and-partitions-count/](https://www.conduktor.io/kafka/kafka-topics-choosing-the-replication-factor-and-partitions-count/)