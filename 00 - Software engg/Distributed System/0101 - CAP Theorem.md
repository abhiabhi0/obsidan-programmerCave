The **CAP Theorem**, also known as **Brewer's Theorem**, is a fundamental principle in distributed computing that states that a distributed data store can only provide two of the following three guarantees simultaneously:

1. **Consistency (C)**: All nodes see the same data at the same time. When a read operation is performed, it returns the most recent write or an error.
2. **Availability (A)**: Every request received by a non-failing node in the system must result in a response, ensuring that the system remains operational.
3. **Partition Tolerance (P)**: The system continues to operate even in the presence of network partitions or failures.

### Key Points of CAP Theorem

- **Trade-offs**: In the event of a network partition, a system must choose between consistency and availability. This means that if a partition occurs, you can either ensure that all nodes return the latest data (consistency) or allow some nodes to respond with potentially outdated data (availability) but not both.
- **Real-world implications**: No distributed system can completely avoid network failures; thus, understanding how to balance these properties is crucial for system design.

### Diagram Representation

Here's a simple Venn diagram representation of the CAP theorem:

```
       +------------------+
       |                  |
       |      C          |
       |                  |
       |      +----------+----------+
       |      |                     |
       |      |         A           |
       |      |                     |
       +------+---------------------+
              |        P            |
              +---------------------+
```

### Types of Systems Based on CAP

Based on which properties are prioritized, systems can be categorized as follows:

- **CP Systems (Consistency and Partition Tolerance)**: These systems ensure that all nodes have consistent data even during network partitions but may sacrifice availability. An example is traditional relational databases like PostgreSQL.
  
- **AP Systems (Availability and Partition Tolerance)**: These systems prioritize availability and will return responses even if they cannot guarantee consistency during partitions. Examples include NoSQL databases like Cassandra and DynamoDB.

- **CA Systems (Consistency and Availability)**: These systems provide both consistency and availability under normal conditions but cannot handle network partitions effectively. Such systems are theoretical and not practically realizable in distributed environments.
[[0300 - CAP Theorem, PACELC Theorem, and Master-Slave Systems]]
### Practical Considerations

When preparing for your interview, consider these practical aspects:

1. **Choosing Properties**: Be prepared to discuss scenarios where you would prioritize one property over another based on application requirements.
2. **Real-World Examples**: Familiarize yourself with different databases and their approach to the CAP theorem:
   - **Cassandra**: AP system prioritizing availability.
   - **MongoDB**: Offers tunable consistency levels, allowing choices between CP and AP.
   - **HBase**: Generally CP, focusing on strong consistency.

3. **PACELC Theorem**: An extension of CAP, which states that even when there is no partition, you still have to choose between latency and consistency. This theorem adds another layer of decision-making for system design.

### Interview Preparation Tips

- **Understand Trade-offs**: Be ready to explain how different choices affect system behavior and user experience.
- **Use Diagrams**: When discussing systems, use diagrams to illustrate your points clearly.
- **Stay Updated**: Keep abreast of current trends in distributed databases and their handling of CAP properties.

By mastering these concepts and being able to articulate them clearly, you will demonstrate a solid understanding of distributed systems during your interview. Good luck!

Citations:
[1] https://www.bmc.com/blogs/cap-theorem/
[2] https://www.tryexponent.com/blog/cap-theorem
[3] https://daily.dev/blog/cap-theorem-explained-consistency-availability-partition-tolerance
[4] https://www.geeksforgeeks.org/5-common-system-design-concepts-for-interview-preparation/
[5] https://www.educative.io/blog/what-is-cap-theorem
[6] https://www.youtube.com/watch?v=AJ11dV1dYqI
[7] https://en.wikipedia.org/wiki/CAP_theorem
[8] https://news.ycombinator.com/item?id=34999464
[9] https://www.geeksforgeeks.org/the-cap-theorem-in-dbms/