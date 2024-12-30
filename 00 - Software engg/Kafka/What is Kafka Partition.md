
1. **Partition Definition**:
    
    - A **partition** is a **subsection** of a Kafka topic. Think of a topic as a "logical channel" where data/messages are sent, and partitions are "physical chunks" that split this channel.
    - Each partition is stored **separately** across Kafka brokers, making Kafka **scalable** and **fault-tolerant**.
2. **Characteristics**:
    - **Immutable Log**: Each partition is an append-only, ordered sequence of records. Once a record is written, it cannot be modified or deleted until the retention period expires.
    - **Sequential Offsets**: Messages in a partition are assigned a unique, sequential offset that identifies their position in the partition.