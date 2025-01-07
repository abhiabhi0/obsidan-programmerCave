### **1. Problem Discussion**

- **Design Goal**: Manual sharding system supporting:
    - Addition/removal of machines.
    - Even distribution of load and storage.
    - Maintaining configuration settings (e.g., replication level).
    
- **Challenges of the Current Approach (Sharding Key: First letter of username)**:
    - Uneven load distribution (e.g., more usernames starting with 'a' than 'x').
    - Upper limit of shards (26 for English alphabets).
    - Scalability issues as the dataset grows.

---

### **2. Volunteer’s Input**

- Proposed Solution: Add new machines dynamically when load exceeds a threshold.
- **Drawbacks**:
    - Sharding at peak time adds to load, degrading system performance.
    - Peak traffic patterns make real-time sharding inefficient.

---

### **3. Consistent Hashing Recap**

- **Process**:
    - Assign hash values to shards using multiple hashing functions.
    - User IDs (or sharding keys) are hashed to determine the shard.
    - New shards (e.g., S4) redistribute load uniformly by taking users from existing shards.
- **Implementation**:  
    The circle metaphor represents a sorted array where each shard occupies a range.
    **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdWXoj9mULGuiL9P48uTJYlP9OD024Id1-efj-Fz-2SMVu_xJn6Vg9SCBdHeC4UnEfrghf-0KWEzTpWjVebA09VTgy_6lAJ5fX4EPV8CT7BM2uuLnJFGWUpHnj3AxoeFX6T2D78UWwr03Jkd3zB49hwlzSr?key=HB8ReJMby42lJxsVnu1bdA)**

---

### **4. Manual Sharding (SulochanaDB custom name)**

- **Properties**:
    - Initially empty with shards S1, S2, and S3.
    - Each shard has 1 master and 2 slaves.
    - Clients use consistent hashing to route requests.
- **Read Behavior**: Any of the three machines can handle reads.
- **Write Behavior**: Depends on whether the system is highly available or consistent.
    **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfjj_omz6k_70RPbmM7pdgDY8qeZ-yMltckrz1onRlrCi7PQKOKhO19rRs3_fdXjqF0fvAu3vry3Vw9SiEPTt8qVoQ7QSsHJfgWa5VAZtOC1sKAlcsci2_tVE2IzFnq6qx-eLddL_41mIVSsdvrhJbxhjoF?key=HB8ReJMby42lJxsVnu1bdA)**
---

### **5. Open Questions**

1. How to handle the addition or removal of machines?
2. How to maintain replication levels if machines fail?
3. Where to place new machines:
    - Add to an existing shard.
    - Keep in standby mode.
    - Create a new shard.

---

### **6. Custom Algorithm**

- **Priorities**:
    1. Maintain replication level by replacing crashed machines.
    2. Create new shards with remaining machines.
    3. Use machines in standby mode.
    
- **Modifications**:
    - Maintain a threshold number of machines XX in standby.
    - Use orchestration systems (e.g., NameNode, JobTracker) to manage these transitions.

---

### **7. Utilization of Standby Machines**

- Serve as additional replicas in existing shards.
- Seamlessly replace failed replicas without additional intervention.

---

### **8. Seamless Shard Creation**

- **Staging Phase**:
    
    - New shard is not added to the hashing circle yet.
    - Simulation determines the hash ranges allocated to the new shard.
    - Data is copied but may lack writes made during this phase.
- **Real Phase**:
    
    - New shard goes live.
    - Missing data (delta updates) is quickly synced.
- **Timelines**:
    
    - **T1**: Staging begins.
    - **T2**: New shard is live.
    - **T3**: Delta updates are complete.

---

### **9. Estimate of Reserved Machines**

- Formula: **Reserved Machines = X * Number of Shards**.
- Depends on:
    - Maximum number of dead machines at a time.
    - Machine quality and average age.

---

### **10. Multi-Master Systems**

- All machines act as masters (no slaves).
- Data is replicated in cyclic order to maintain consistency.
- Example: For replication level X=3X = 3, U1’s data is stored in M1, M2, and M3.

---

### **11. Read and Write Operations**

- **Tunable Consistency**:
    
    - RR: Minimum reads for a successful read operation.
    - WW: Minimum writes for a successful write operation.
    - R+W>XR + W > X: Highly consistent.
    - R+W≤XR + W \leq X: Tradeoff between availability and consistency.
- **Examples**:
    
    - **Highly Consistent**: R=1,W=3R = 1, W = 3.
    - **Highly Available**: R=1,W=1R = 1, W = 1.

---

### **12. Questions for Next Class**

- How to handle variable-sized records in NoSQL DBs?
- Addressing storage structure challenges for Key-Value, Document, and Column-Family DBs.

---

### **13. Complexity of Write and Delete Operations**

- **Delete**: Faster as it removes references, not actual data.
- **Write**: Slower as it overwrites every bit involved.

https://docs.google.com/document/d/1PfCmn-rhCJEyWyWf45VtIUFRHyoqiwFyp5o2TCcrZnE/edit?tab=t.0