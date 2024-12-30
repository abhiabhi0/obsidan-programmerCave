The **Sticky Assignor** is a partition assignment strategy in Kafka designed to minimize **unnecessary rebalancing** while maintaining a balanced distribution of partitions among consumers in a consumer group.

---

### **Purpose of the Sticky Assignor**

1. **Reduce Partition Movement**:
    
    - During rebalancing, it attempts to **retain the existing partition assignments** for consumers as much as possible.
    - This reduces the disruption caused by frequent partition reassignments.
2. **Ensure Balanced Distribution**:
    
    - While retaining assignments, it also ensures that the partitions are distributed as evenly as possible across all consumers.
3. **Optimize Consumer Performance**:
    
    - By avoiding unnecessary reassignments, the Sticky Assignor minimizes the performance impact of rebalancing, as reassignments can temporarily disrupt message processing.

---

### **How Does the Sticky Assignor Work?**

1. **Retention of Previous Assignments**:
    
    - During a rebalance, the Sticky Assignor checks the current partition assignments.
    - It tries to keep as many partitions assigned to their existing consumers as possible, as long as doing so doesn't violate the balance of partition distribution.
2. **Balancing the Load**:
    
    - If a new consumer joins or leaves the group, it redistributes partitions only as necessary to ensure an even workload.
    - For example:
        - If there are 9 partitions and 3 consumers, the Sticky Assignor ensures that each consumer gets 3 partitions, while minimizing changes to the previous assignments.
3. **Fallback to Round-Robin**:
    
    - If retaining previous assignments conflicts with achieving balance (e.g., if partitions can't be evenly distributed), the Sticky Assignor falls back to a fair redistribution, similar to the **RoundRobinAssignor**.

---

### **Comparison with Other Assignors**

|Feature|Range Assignor|Round Robin Assignor|Sticky Assignor|
|---|---|---|---|
|**Partition Retention**|Not prioritized|Not prioritized|Prioritized|
|**Balancing of Partitions**|May result in imbalances|Always balanced|Always balanced|
|**Rebalancing Overhead**|Moderate to high|Moderate|Minimal|
|**Use Case**|Contiguous assignment|Fair distribution|Balanced & sticky|

---

### **Example of Sticky Assignor Behavior**

#### Initial Assignment:

- **Topic**: "orders" with 6 partitions (P0–P5).
- **Consumers**: C1, C2.
- **Assignment**:
    - C1 → P0, P1, P2
    - C2 → P3, P4, P5

#### A New Consumer Joins (C3):

- The Sticky Assignor minimizes changes:
    - C1 → P0, P1
    - C2 → P3, P4
    - C3 → P2, P5

#### A Consumer Leaves (C1):

- The Sticky Assignor redistributes partitions:
    - C2 → P3, P4
    - C3 → P0, P1, P2, P5

In both cases, only the necessary changes are made to maintain balance, minimizing disruption.

---

### **Advantages of the Sticky Assignor**

1. **Minimized Disruption**:
    
    - By retaining existing assignments, consumers don’t need to reinitialize or reprocess partitions unnecessarily.
2. **Better Performance**:
    
    - Reducing partition movement decreases downtime during rebalancing, improving overall throughput.
3. **Ideal for Stateful Applications**:
    
    - In cases where consumers maintain local state for a partition (e.g., caching or aggregations), retaining assignments avoids costly state reconstruction.

---

### **Use Cases**

1. **Real-Time Analytics**:
    - Systems processing large volumes of events with minimal tolerance for downtime benefit from reduced rebalancing.
2. **Stateful Stream Processing**:
    - Applications using Kafka Streams or similar frameworks rely on sticky assignments to maintain partition-to-state consistency.
3. **Low-Latency Systems**:
    - Systems requiring minimal disruption during consumer group changes.

---

### **Configuration**

You can configure the Sticky Assignor in Kafka by setting the `partition.assignment.strategy` property in the consumer configuration:

```properties
partition.assignment.strategy=org.apache.kafka.clients.consumer.StickyAssignor
```

This ensures that Kafka uses the Sticky Assignor for partition assignments.

---

### **Summary**

The **Sticky Assignor** is a powerful Kafka feature that:

- Prioritizes retaining existing partition assignments during rebalancing.
- Ensures an even and balanced distribution of partitions among consumers.
- Reduces the overhead and disruption caused by frequent reassignments. It is particularly beneficial in applications where stability and low-latency processing are critical.