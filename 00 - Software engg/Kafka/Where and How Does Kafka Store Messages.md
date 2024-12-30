### Where?

Kafka stores messages in a **distributed log system**. These logs are stored on the disks of Kafka brokers in the Kafka cluster.

1. **Topics and Partitions**:
    - Each **topic** in Kafka is divided into multiple **partitions**.
    - A partition is a physical directory on disk where messages are stored.
    
1. **Broker Storage**:
    - Each Kafka **broker** is responsible for storing the partitions assigned to it.
    - Partitions are replicated across brokers for fault tolerance, and the replicas are distributed to balance the load.
    
1. **Filesystem and Storage Path**:
    - Kafka uses the local **filesystem** of the broker.
    - By default, messages are stored under a directory configured by the `log.dirs` property in Kafka’s configuration. For example:  
        `/tmp/kafka-logs/<topic-name>-<partition-number>/`
    - Each partition is represented as a directory containing log files and metadata.

---
![[{C27387EA-B357-4568-9CD2-058F436D5812}.png]]
### How?

Kafka uses **segmented log files** and an **append-only log structure** for efficient storage and retrieval.

1. **Append-Only Log**:
    - Messages are written sequentially to disk in an **append-only fashion**.
    - This design optimizes I/O performance, taking advantage of the disk's sequential read/write capabilities.

1. **Message Segmentation**:
    - Kafka splits each partition’s log into **segments**.
    - A segment is a fixed-size file (default: 1GB, configurable via `log.segment.bytes`).
    - Each segment is named using the offset of the first message in the segment, e.g., `00000000000000000000.log`.
    
1. **Indexes for Fast Retrieval**:
    - Along with the log files, Kafka maintains **index files** for each segment:
        - **Offset Index**: Maps message offsets to their positions in the log file.
        - **Timestamp Index**: Maps timestamps to message offsets.
    - These indexes allow Kafka to retrieve messages efficiently without scanning the entire log.
    
1. **Retention and Cleanup**:
    - Kafka retains messages for a configurable **retention period** (default: 7 days, configurable via `log.retention.hours`) or until the total log size exceeds a limit (`log.retention.bytes`).
    - When the retention period or size limit is exceeded, older log segments are deleted or compacted based on the configured cleanup policy (`delete` or `compact`).

---
### **Why Disk and Not Memory?**

Kafka uses disk-based storage instead of memory for the following reasons:
1. **Durability**:
    - Messages persist on disk, ensuring they are not lost if the broker crashes.
2. **Scalability**:
    - Disk-based storage allows Kafka to handle large volumes of data that wouldn’t fit in memory.
3. **Cost Efficiency**:
    - Disk storage is cheaper than memory, making Kafka cost-effective for handling massive data streams.
