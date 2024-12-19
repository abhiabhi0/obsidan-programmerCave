![[{8A8DDE8D-1119-4ABC-AF90-4B4C908F4D7D}.png]]

### What is Database Sharding?
- Database sharding is a technique of horizontally partitioning a database into smaller, more manageable pieces called "shards."
- Each shard is an independent database that contains a subset of the overall data.
- It is used to enhance performance, scalability, and availability.

---
### Key Concepts:
1. **Horizontal Partitioning**:
    - Divides rows of a table into multiple smaller tables.
    - Each table (shard) contains the same schema but different rows of data.
2. **Shard Key**:
    - A column or set of columns used to determine which shard a particular piece of data belongs to.
    - Example: Customer ID, Order ID.

---
### Advantages:
1. **Scalability**:
    - Distributes data across multiple servers, allowing for linear scaling.
    - Enables handling of larger datasets as the application grows.
1. **Performance**:
    - Queries and operations target a specific shard, reducing the load on individual servers.
    - Minimizes latency for data access.
1. **Fault Isolation**:    
    - Failure in one shard does not impact the others, enhancing availability.
1. **Cost Efficiency**:
    - Allows the use of commodity hardware instead of investing in a single high-performance machine.
---
### Challenges:
1. **Complexity**:
    - Application logic must account for shard placement and query routing.
    
1. **Data Distribution**:
    - Uneven distribution (hotspots) can lead to overloaded shards.
    
1. **Cross-shard Queries**:
    - Complex and resource-intensive as data resides on multiple shards.

1. **Data Rebalancing**:    
    - Adding or removing shards requires redistributing data, which can be complex.

---
### Sharding Strategies:
1. **Range-Based Sharding**:
    - Data is divided based on ranges of the shard key.
    - Example: Users with IDs 1-1000 go to Shard 1, 1001-2000 to Shard 2.
    - Pro: Simple to implement.
    - Con: Risk of unbalanced shards if data is not evenly distributed.
1. **Hash-Based Sharding**:
    - A hash function determines the shard for each data entry.
    - Pro: Better distribution of data.
    - Con: Harder to rebalance shards dynamically.
1. **Geographic Sharding**:    
    - Data is partitioned based on geographical location.
    - Example: Users in Asia go to Shard 1, Europe to Shard 2.
    - Pro: Reduces latency for users in specific regions.
    - Con: Complex when users span multiple regions.

---
### When to Use Sharding?
- The database cannot handle the read/write operations efficiently on a single server.
- The dataset size exceeds the storage capacity of a single machine.
- Specific shards can be deployed closer to users for low-latency access.

---
### Examples of Sharded Systems:
1. **Social Media Platforms**:
    - User profiles or posts are sharded by user ID.
2. **E-commerce Websites**:
    - Orders and products are sharded by region or category.
3. **Banking Systems**:
    - Transactions are sharded by account number.

#### Real-World Use Case:

- Instagram shards its user database by user ID to manage the billions of accounts efficiently.