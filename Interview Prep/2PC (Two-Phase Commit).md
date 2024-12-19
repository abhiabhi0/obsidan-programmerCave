The **Two-Phase Commit (2PC)** is a distributed algorithm used to ensure **atomicity** in a distributed transaction, meaning the transaction either commits fully across all nodes or rolls back entirely. It ensures all participating nodes in a distributed system agree on the outcome of a transaction.

---

### **Phases in 2PC**

#### **1. Phase 1: Prepare Phase**

- The **coordinator** sends a `Prepare` message to all participating nodes (called participants) to check if they can commit the transaction.
- Each participant:
    1. Prepares to commit by locking the resources required for the transaction.
    2. Writes the transaction to a local log.
    3. Responds with:
        - `Vote-Commit`: If it can commit.
        - `Vote-Abort`: If it encounters an issue and cannot commit.

#### **2. Phase 2: Commit/Abort Phase**

- Based on the responses, the coordinator decides:
    1. If **all participants voted to commit**:
        - The coordinator sends a `Commit` message to all participants.
        - Participants commit the transaction and release the locks.
    2. If **any participant voted to abort**:
        - The coordinator sends an `Abort` message to all participants.
        - Participants roll back the transaction and release the locks.

---

### **Detailed Steps**

1. **Begin Transaction**:
    
    - The coordinator identifies the nodes involved in the transaction.
2. **Phase 1 (Prepare)**:
    
    - Coordinator sends a `Prepare` request to all participants.
    - Participants respond with either `Vote-Commit` or `Vote-Abort`.
3. **Phase 2 (Decision)**:
    
    - If all participants respond with `Vote-Commit`:
        - Coordinator sends a `Commit` message.
        - Participants commit and release resources.
    - If any participant responds with `Vote-Abort`:
        - Coordinator sends an `Abort` message.
        - Participants roll back and release resources.
4. **End Transaction**:
    
    - Once all participants acknowledge the commit or abort, the transaction ends.

---

### **Advantages of 2PC**

1. **Atomicity**: Guarantees that a transaction either fully commits or fully aborts across all nodes.
2. **Consistency**: Ensures the database remains in a valid state.

---

### **Disadvantages of 2PC**

1. **Blocking Protocol**:
    - Participants must hold locks until they receive the final decision, which can lead to **resource contention**.
2. **Coordinator Failure**:
    - If the coordinator fails during the process, participants might be left in an uncertain state.
3. **High Latency**:
    - The protocol involves multiple round trips, which can cause delays in high-latency networks.
4. **No Fault Tolerance**:
    - It doesn’t handle failures gracefully, as a failure in any participant or the coordinator may result in aborts.

---

### **Use Case Example**

#### Scenario: Banking System

- **Transaction**: Transfer $100 from **Account A** (on Node 1) to **Account B** (on Node 2).
- **Phase 1**:
    - Coordinator asks Node 1 and Node 2 to prepare:
        - Node 1 locks $100 from Account A.
        - Node 2 locks resources for depositing $100.
    - Both nodes respond with `Vote-Commit`.
- **Phase 2**:
    - Coordinator sends `Commit`:
        - Node 1 deducts $100.
        - Node 2 credits $100.
        - Locks are released.

---

### **Alternatives to 2PC**

1. **Three-Phase Commit (3PC)**:
    - Adds a pre-commit phase to reduce the risk of blocking in case of a coordinator failure.
2. **Consensus Protocols**:
    - Use protocols like **Paxos** or **Raft** for fault tolerance in distributed systems.

---

### **Key Interview Takeaways**

- **Why Use 2PC?**
    - Ensures atomicity in distributed transactions where multiple systems are involved.
- **What Are Its Weaknesses?**
    - Blocking nature and inability to handle coordinator failure gracefully.
- **How Does It Differ from 3PC?**
    - 3PC adds a timeout-based pre-commit phase to handle failures better.

Let me know if you'd like any specific examples or additional clarification!