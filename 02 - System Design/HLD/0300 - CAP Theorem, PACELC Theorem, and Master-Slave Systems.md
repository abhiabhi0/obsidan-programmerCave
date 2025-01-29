### **1. CAP Theorem**

The CAP theorem states that a distributed system can only provide two of three properties simultaneously:

- **Consistency (C):** Every read receives the most recent write or an error.
- **Availability (A):** Every request receives a response, without guarantee that it contains the most recent write.
- **Partition Tolerance (P):** The system continues to operate despite network partitions.
[[0101 - CAP Theorem]]
#### **Real-life Example:**

Rohit starts a company, "Reminder," where he notes reminders for callers. Initially, Rohit handles all calls and records them in a diary. As the business grows, he hires Raj to manage the workload. This leads to:

- **Problem 1: Inconsistency**
    - X’s reminder is recorded by Raj but not Rohit, causing inconsistent data.
    - **Solution:** Both write entries simultaneously to ensure consistency.
    - **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcZOxPU832-hUQ6U_sD1Qkn5MZuqQuF2s1OajVfRzLd7UQgqudME9lzeAyQl2Na-FanHnrRfN8aY2ogRw-jMgrt6Ww6SMm1XaipN3UNhEVBmt-UhnoJNek8-PvRSgjr_U4VWG4UvaD-61Ryycb2vB_Lr5Gu?key=CmWYzhsGOSF0-GDHo3DPgg)**

- **Problem 2: Availability Issue**
    - Raj’s absence leads to delays as both must write entries before returning success.
    - **Solution:** Accept entries even if one is absent, and ensure synchronization before resuming operations.
    - **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcGr6QENPh6r_PS97VTrC_RxZdqgWMiJc8tSD_z4Xgh-NzeYVXKk9VI_QvzzCraSs7KtSWAfKlYTj3LBvI-_DRMEIfWTThCvmc-0Qt1MVwChTZl4eSl9LpO8hopObt8PlDG70KfKJEMsJw7o8b5uhVZT9zC?key=CmWYzhsGOSF0-GDHo3DPgg)**

- **Problem 3: Network Partition**
    - If Raj and Rohit stop communicating, either:
        - Accept entries and risk inconsistency.
        - Reject entries to maintain consistency, reducing availability.
        - **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfXdJKsErXJ5h1NbPBqlJhwpVWKDvQa0ocIfULMyuVKfNP0mb5SE5sa_ZNaiN6vRFU1ki6E_d0MtZgVYm7-h0enO0fCZjOA62qyt3h6Ye3_teC8o2RqeuHIHQHacjN5KWwOUiU2fK7einzyU4EnTvXMZaOC?key=CmWYzhsGOSF0-GDHo3DPgg)**

**Key Insight:** In the event of a partition, systems must choose between **Consistency** and **Availability.**

---

### **2. PACELC Theorem**

The PACELC theorem builds on CAP by addressing system behavior in normal conditions:

- **P:** During a partition, choose between **Availability (A)** and **Consistency (C).**
- **ELC:** Else, in the absence of partitions, choose between **Latency (L)** and **Consistency (C).
- **![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXep0uAT8jHG9QFhw_De13TCQBgua-cFcJubpyRyAWRwkdKqGYgIFSQ29x3fWI4Y5IXwq7uGQUCMogncXoGM4QtrFPGgv4JExdROqIwjRPCeDaQUWLit1iDEcobhdHt3A9dCCUdtAZ6xJ6apO-bOKNxb6Rzy?key=CmWYzhsGOSF0-GDHo3DPgg)****

#### **Trade-offs:**

- **Banking Systems:** High consistency is critical for transactions.
    - Example: ATM transactions may use eventual consistency but aim for immediate consistency when possible.
- **Social Media (e.g., Facebook News Feed):** Availability is prioritized over consistency.
- **Messaging Systems (e.g., Facebook Messenger):** High consistency is crucial to avoid miscommunication.

#### **Key Takeaway:**

Choose trade-offs based on use case requirements, balancing consistency, availability, and latency.

---

### **3. Master-Slave Systems**

In Master-Slave architecture:

- One machine acts as the **Master** to handle writes.
- Other machines (Slaves) replicate data from the Master.

#### **3.1 Types of Master-Slave Systems**

1. **Highly Available, Not Eventually Consistent:**
    - **Steps:** Master writes data and returns success immediately. Slaves sync asynchronously.
    - **Example:** Log processing systems like Splunk prioritize throughput and can tolerate some data loss.
    
1. **Highly Available, Eventually Consistent:**
    - **Steps:** Master writes data. If one slave confirms, success is returned. Other slaves sync later.
    - **Example:** Social media news feed systems prioritize availability but ensure eventual data consistency.
    
1. **Highly Consistent:**
    - **Steps:** Master and all slaves write data. Success is returned only after all writes are complete.
    - **Example:** Banking systems where consistency is critical.

#### **Operational Details:**

- **Writes:** Always go to the Master.
- **Reads:** Can go to any machine (Master or Slaves).
- **Master Failure:** A new Master is elected using algorithms like Paxos or Raft.

---

#### **3.2 Drawbacks of Master-Slave Systems**

1. **Bottleneck at Master:**
    - Single Master limits scalability for high write loads.
    
1. **Increased Latency in Highly Consistent Systems:**
    - As the number of slaves grows, synchronization delays increase.
    
1. **Scalability Limitations:**
    - Systems with a large number of slaves (e.g., 1000) face higher failure rates and require sharding for better performance.

---
https://docs.google.com/document/d/1e3cKPLd8ajBEJ17HK6IAn7T1qP4QWSJ9cN3DHn3X_x0/edit?tab=t.0
### **Additional Reading and Resources**

- Martin Kleppmann’s blog on CAP theorem: [Stop Calling Databases CP or AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
- Google Spanner and CAP theorem: [Spanner Research Paper](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/45855.pdf)
- Video Resources:
    - [CAP Theorem Simplified](https://www.youtube.com/watch?v=oeycOVX70aE)
    - [Understanding PACELC](https://www.youtube.com/watch?v=LaLT6EC7Trc)