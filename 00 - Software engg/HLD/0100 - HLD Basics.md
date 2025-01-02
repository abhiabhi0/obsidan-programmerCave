### **System Design Overview**

- **What is System Design?**
    - **Low-Level Design (LLD):** Focuses on the internal structure of individual components or code within a single service.
    - **High-Level Design (HLD):** Focuses on the design of multiple components/services to achieve a scalable and efficient system.

---

### **Distributed Systems**

#### **Why are Distributed Systems Needed?**

1. **Initial Setup:**
    - Example: Del.icio.us started as a bookmarking service on a single laptop.
    - Initial setup involves implementing simple functions like:
        - `addBookmark(userId, site_url)`
        - `getAllBookmarks(userId)`
        
1. **Challenges in Scaling:**
    - **Domain Resolution:**
        - Domain names need to resolve to IP addresses via a system like DNS.
        - DNS maintains decentralized copies of centrally stored data to prevent single points of failure.
        - [[What happens when you type a URL into your browser]]
        
    - **Downtime During Deployment:**
        - Load balancers can route traffic to healthy machines and handle requests evenly during updates.
        - [[How to deploy in production such that background jobs are not stopped]]
        - [[0101 - Load Balancer]]
        - [[API Gateway]]
        - [[0201 - API Gateway vs Ingress]]
        
    - **Storage Constraints:**
        - Sharding is used to distribute data across multiple machines when storage becomes a bottleneck.
        - [[Database Sharding]]


[[Top 7 Most-Used Distributed System Patterns]]

---
### **Key Concepts in Distributed Systems**

#### **DNS (Domain Name System):**

- Maps domain names to IP addresses.
- DNS servers store a replicated copy of central data from ICANN and update periodically.
#### **Load Balancers:**

- Split traffic among multiple servers and handle failovers.
- Techniques:
    - Health checks/heartbeats to track server availability.
    - Load balancing algorithms: Round-robin, Weighted Round-robin, IP Hashing.
    - [[Types of load balancing algorithms]]

#### **Sharding:**

- Splitting data across multiple machines based on a **sharding key**.
    - **Example:** Using `user_id` ensures all a user’s bookmarks stay on one machine.
- **Constraints:**
    - Lightweight mapping of user IDs to shards.
    - Avoid load skew and ensure even data distribution.
    - Support dynamic shard addition/removal with minimal downtime.
    - [[Database Sharding]]

---

### **Sharding Approaches**

1. **Modulo-Based Sharding:**  
    `userId % number_of_shards`
    
    - Pros: Simple.
    - Cons: Major data reshuffling when shards are added/removed.
2. **Range-Based Sharding:**
    
    - Pros: Easy to understand.
    - Cons: Load skew as user activity grows unevenly across ranges.
3. **Consistent Hashing:**
    
    - Maps user IDs to a circular hash space and assigns users to the nearest machine in the cycle.
    - **Improved Design:** Use multiple hash points per machine to distribute load evenly when a shard fails.

---

### **Consistent Hashing Details**

- Nodes are assigned multiple positions on the hash ring using different hash functions.
- Benefits:
    - Failure of one node spreads its load across multiple other nodes, avoiding cascading failures.
    - Helps in balancing load when nodes are added/removed.
    - [[0102 - Consistent Hashing]]
**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfDkjUHtWULFZ5agnplJ-gIHvHaUbNL2bA0_TxyyBpAJfj5ZyDkZTb9gwet93SQ0FZsyIyZ0_0zSp_zqK9KH_YwmJ4ONHTvza8l_4bKOVag3GvC-YuWB1eeAZ0IeT9YsmaNFh4JKLhltSEUM9IQmiRceac?key=P2WYkAHXMMX2Am3ZvvrbMg)**

https://docs.google.com/document/d/1wY6rSGJwqa9uxAsoYZUQ0FumV3Sp5kRBCknUQPT3gcM/edit?tab=t.0