### **SQL Database**

#### **Features**

1. **Normalization**:
    - Reduces data redundancy by ensuring consistent data across tables.
    - Example: If a student's score is updated in one table, normalization ensures that all related tables reflect the same score.
    - [[Database Normalization]]
    
1. **ACID Transactions**:
    - **Atomicity**: Transactions are all-or-nothing (e.g., either money is transferred, or the entire transaction is rolled back).
    - **Consistency**: Ensures the database remains valid before and after a transaction.
    - **Isolation**: Prevents one transaction from interfering with another (e.g., simultaneous withdrawals).
    - **Durability**: Changes persist even after crashes.
    - [[Differences Between Atomicity and Consistency]]
    - [[Isolation Levels in ACID Properties]]

1. **Defined Schema**:    
    - Fixed columns and data types, ensuring data consistency and validation.
    - Example: A `Users` table might define `Name` as a `VARCHAR(50)` and `Age` as an `INTEGER`.

#### **Shortcomings**

1. **Fixed Schema**:
	Let’s design the schema for an ecommerce website and just focus on the Product table. There are a couple of pointers here:
	
	- Every product has a different set of attributes. For example, a t-shirt has a collar type, size, color, neck-type, etc.. However, a MacBook Air has RAM size, HDD size, HDD type, screen size, etc. 
	- These products have starkly different properties and hence couldn’t be stored in a single table. If you store attributes in the form of a string, filtering/searching becomes inefficient.
	- However, there are almost 100,000 types of products, hence maintaining a separate table for each type of product is a nightmare to handle. 

_SQL is designed to handle millions of records within a single table and not millions of tables itself._

Hence, there is a requirement of a flexible schema to include details of various products in an efficient manner.
    
1. **Scaling Issues**:
	- If there is a need of sharding due to large data size, performing a SQL query becomes very difficult and costly.
    
	- Doing a JOIN operation across machines nullifies the advantages offered by SQL.
    
	- SQL has almost zero power post sharding. You don’t want to visit multiple machines to perform a SQL query. You rather want to get all data in a single machine.
	
As a result, most SQL databases such as postgresql and sqlite do not support sharding at all.

---

### **NoSQL Databases**

1. **Sharding and Denormalization**:
    - Sharding splits data across multiple machines for scalability.
    - Denormalization replicates data to avoid complex JOINs.
    - Example: Messaging apps often store messages redundantly (sent and received copies) to avoid JOINs.
    
1. **Examples of Choosing a Good Sharding Key**:
    **Banking System**:
    - **Good Key**: UserID (ensures balanced load and localized operations like balance inquiries).
    - **Bad Key**: CityID (causes unbalanced load, especially for large cities).
    
    **Uber-like System**:
    - **Good Key**: CityID (drivers and rides are localized by city).
    - **Bad Key**: DriverID or PIN Code (queries become complex, involving multiple machines).
    
    **Slack (Groups-heavy system)**:
    - **Good Key**: GroupID (group messages are stored together, reducing cross-machine writes).
    - **Bad Key**: UserID (leads to redundant writes across multiple machines).
    
    **IRCTC**:
    - **Good Key**: TrainID (efficient for ticket booking and seat allocation per train).
    - **Bad Keys**: Date or UserID (overloads specific machines during peak times like tatkal bookings).
	
**Few points to keep in mind while choosing Sharding Keys:**
- Load should be distributed uniformly across all machines as much as possible.
- Most frequent operations should be performed efficiently.
- Minimum machines should be updated when the most-frequent operation is performed. This helps in maintaining the consistency of the database.
- Minimize redundancy as much as possible.
    
1. **Types of NoSQL Databases**:
    - **Key-Value DBs**:
        - Data stored as key-value pairs (like a dictionary).
        - Suitable for caching, session management, or real-time data.
        - Examples: Redis, DynamoDB.
        
    - **Document DBs**:
        - Data stored as JSON-like objects, allowing flexible and dynamic structures.
        - Suitable for applications like e-commerce, where products have diverse attributes.
        - Examples: MongoDB, ElasticSearch.
        
    - **Column-Family Storage**:
        - Data grouped by columns rather than rows, enabling efficient range queries and analytics.
        - Examples: Cassandra, HBase.
        
1. **Choosing a Proper NoSQL DB**:
    - **Twitter-HashTag Storage**:
        - **Best Choice**: Column-Family DB (supports efficient search and pagination).
        - **Not Suitable**: Key-Value DB (lacks range-query support).
    - **Live Scores of Sports/Matches**:
        - **Best Choice**: Key-Value DB (lightweight and optimized for frequent updates).
    - **Current Cab Location in Uber**:
        - **Key-Value DB**: Best if only current location is needed.
        - **Column-Family DB**: Best if location history is also required.

https://docs.google.com/document/d/1CfwsMMw8NTn2zavVokiYG08EH648Gsd1zJ7aAM7NvS4/edit?tab=t.0
---

### **Questions for Next Class**

#### **Problem Statement 1**
- How to manage unbounded values in NoSQL DBs?
    - Key-Value DB: No limit on value size.
    - Document DB: Flexible schemas allow many attributes.
    - Column-Family DB: Handles large entries with efficient queries.
#### **Problem Statement 2**
- Design a Manual Sharding System:
    - Dynamically add shards and migrate data.
    - Handle machine failures with replication and recovery.
    - Scale seamlessly as the system grows.
#### **Key Points**:
1. **Facebook Example**:
    - Uses manual sharding for its UserDB (MySQL), involving consistent hashing and recovery mechanisms.
        
2. **SQL for Small Companies**:
    - Startups benefit from SQL simplicity. NoSQL is ideal for scalability challenges.
        
3. **ACID in NoSQL**:
    - Achieved through workarounds like central shard locks or distributed transactions.
        
4. **Transaction States in Banking**:
    - Use intermediate states like `send_initiated` and `send_completed` to ensure consistency.
    - Rollbacks handle failures gracefully.

---

### **Resources**

- **Redis**:
    
    - [Try Redis](https://try.redis.io/)
        
    - [Redis Documentation](https://redis.io/docs/)
        
    - [Redis University](https://university.redis.com/)
        
- **MongoDB**:
    
    - [Mongo Playground](https://mongoplayground.net/)
        
    - [MongoDB Docs](https://www.mongodb.com/docs/manual/)
        
- **Cassandra**:
    
    - [Cassandra Basics](https://cassandra.apache.org/_/cassandra-basics.html)
        
    - [Cassandra Case Studies](https://cassandra.apache.org/_/case-studies.html)