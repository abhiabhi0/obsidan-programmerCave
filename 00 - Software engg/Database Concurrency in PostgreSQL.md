### Summary

#### **Concurrency Techniques**:
- Pessimistic Locking
- Optimistic Locking
- Multi-Version Concurrency Control (MVCC)
#### **Pessimistic Locking**
- **Approach**: Assumes high likelihood of conflicts.
- **Mechanism**: Locks data (rows/tables) to ensure exclusive access during transaction.
- **Pros**: Prevents conflicts upfront.
- **Cons**: Increases blocking, reduces concurrency.
#### **Optimistic Locking**
- **Approach**: Assumes conflicts are rare.
- **Mechanism**: No locks during transaction execution; validates for conflicts at commit time.
- **Pros**: Higher concurrency.
- **Cons**: Risk of conflicts at commit time requiring resolution.
#### **Multi-Version Concurrency Control (MVCC)**
- **Approach**: Provides consistent snapshots of data to each transaction.
- **Mechanism**:
    - Each transaction sees data as it was at the start of the transaction.
    - Read operations are non-blocking.
    - Updates still use **row-level pessimistic locks** to prevent conflicts.
- **Usage**:
    - Works well for **reads** in PostgreSQL.
    - Updates involve locking for data consistency.
**PostgreSQL Locks**:
- Provides various lock types to manage concurrency and ensure multi-user consistency.

---
### Concurrency Techniques

Broadly there are three concurrency techniques available for any database management system, pessimistic, optimistic, and multi-valued concurrency control (MVCC). In this section, I will introduce the techniques.

#### Pessimistic Locking
This concurrency control technique is used in database systems to handle concurrent access to shared data. It takes a cautious approach by assuming that conflicts between transactions are likely to occur, and it prevents conflicts by acquiring locks on database objects (rows or tables). Pessimistic locking ensures exclusive access to data, but it can lead to increased blocking and reduced concurrency compared to optimistic locking approaches.

#### Optimistic Locking
This concurrency control technique takes an optimistic approach by assuming that conflicts between transactions are rare, and it allows transactions to proceed without acquiring locks on database objects during the execution of the entire transaction. Conflicts are validated, detected and resolved only at the time of committing the transaction.

#### Multi Version Concurrency Control (MVCC)
This concurrency control technique is used in database systems to allow concurrent access to shared database objects while maintaining data consistency and isolation. MVCC provides each transaction with a consistent snapshot of the data as it existed at the start of the transaction, even when other transactions modify the data concurrently.

MVCC works very well within PostgreSQL for read operations. When it comes to updating the data, PostgreSQL would still acquire locks (which is pessimistic locking) at row level to ensure data consistency and prevent conflict between concurrent transactions. PostgreSQL provides various types of locks to manage concurrency and ensure data consistency in multi-user environments.