### Overview of Consistent Hashing

Consistent hashing is a distributed systems technique used for partitioning data across multiple nodes (e.g., servers). It helps to minimize data movement when the number of nodes changes, ensuring scalability and fault tolerance.

#### How Consistent Hashing Works:

1. **Hash Ring Formation**:
    
    - A hash function maps both data objects (keys) and nodes to positions on a virtual circular ring (hash ring).
    - The output of the hash function is treated as positions on this ring.
2. **Placing Nodes on the Ring**:
    
    - Each node (identified by its IP address or ID) is hashed to determine its position on the ring.
3. **Placing Data on the Ring**:
    
    - Each key is hashed to find its position on the ring.
    - The key is assigned to the first node found when traversing the ring clockwise from the key's position.
4. **Node Addition and Removal**:
    
    - Adding a new node affects only the keys that fall within the range of the new node.
    - Removing a node moves its keys to the next node in the clockwise direction, leaving other nodes unaffected.

#### Key Benefits:

- Scales horizontally with minimal data movement.
- High availability by ensuring data redistribution during node addition/removal.
- Fault tolerance by enabling data replication on adjacent nodes.

---

### Terminology

- **Node**: A server providing functionality in the system.
- **Hash Function**: Maps data of arbitrary size to fixed-size values.
- **Data Partitioning**: Distributing data across nodes to improve performance.
- **Hotspot**: A node handling a disproportionate amount of load.
- **Virtual Nodes**: Technique to distribute load evenly across nodes by assigning multiple positions on the ring to each physical node.

---

### Steps in Consistent Hashing

1. **Node Positioning**:
    
    - Hash each node’s identifier using a hash function (e.g., MD5, SHA-256).
    - Convert the hash output to a position on the ring.
2. **Data Placement**:
    
    - Hash the key of each data object.
    - Traverse the ring clockwise from the key’s position to find the first node.
    - Store the data object on this node.
3. **Data Retrieval**:
    
    - Hash the key of the data object.
    - Traverse the ring clockwise to locate the node responsible for the key.
    - Retrieve the data from this node.

---

### Challenges Addressed by Consistent Hashing

1. **Dynamic Load Handling**:
    
    - When nodes are added or removed, only a subset of keys is reassigned.
    - Unlike static hashing (e.g., modulo N), it avoids remapping all keys.
2. **Hotspots**:
    
    - Virtual nodes distribute load evenly, preventing hotspots.

---

### Implementation Details

- **Data Structure**: A self-balancing Binary Search Tree (BST) stores node positions, enabling O(log n) operations.
- **Operations**:
    - **Adding a Node**:
        1. Insert node position into BST.
        2. Reassign keys from the successor node to the new node.
    - **Removing a Node**:
        1. Delete node position from BST.
        2. Reassign keys to the successor node.
    - **Adding/Removing a Key**:
        - O(log n) for BST traversal.

### Asymptotic Complexity

|Operation|Time Complexity|Description|
|---|---|---|
|Add a node|O(k/n + log n)|Reassign keys + BST update.|
|Remove a node|O(k/n + log n)|Reassign keys + BST update.|
|Add/Remove a key|O(log n)|BST traversal.|

---

### Virtual Nodes

Virtual nodes are created by hashing each node’s ID multiple times to assign it multiple positions on the ring. This improves load balancing by:

- Ensuring even distribution of keys.
- Handling heterogeneous nodes (assigning more virtual nodes to higher-capacity physical nodes).

#### Benefits:

- Uniform load distribution.
- Fault tolerance during node downtimes.

#### Challenges:

- Increased memory costs and complexity.
- Downtime of a virtual node impacts multiple nodes.

---

### Examples of Real-World Use Cases

- **Discord**: Load balancing across chat servers.
- **Amazon DynamoDB**: Dynamic partitioning in NoSQL data stores.
- **Netflix**: Distributing video content across a CDN.
- **Vimeo**: Load balancing video streaming traffic.
- **Memcached**: Ketama clients use consistent hashing for efficient caching.

---

### Advantages and Drawbacks

#### Advantages:

- Scalability with minimal data movement.
- Efficient partitioning and replication.
- Uniform load distribution (with virtual nodes).

#### Drawbacks:

- Cascading failures due to hotspots (popular data objects).
- Increased complexity with virtual nodes.
- Replication logic is more complex.

---

### Common Hash Functions for Consistent Hashing

- **Cryptographic**: MD5, SHA-1, SHA-256 (secure but slower).
- **Non-Cryptographic**: MurmurHash, xxHash, MetroHash (faster, suitable for consistent hashing).

---

### Handling Concurrency

When multiple nodes are added/removed simultaneously, consistency is ensured using a readers-writer lock to synchronize the BST. This prevents race conditions while maintaining performance.

---

Consistent hashing ensures that distributed systems can scale dynamically while maintaining performance and availability. By balancing data efficiently, it’s a foundational technique used in modern scalable architectures.

https://highscalability.com/consistent-hashing-algorithm/
https://www.geeksforgeeks.org/consistent-hashing/