Concurrency control protocols are the set of rules which are maintained in order to solve the concurrency control problems in the database. It ensures that the concurrent transactions can execute properly while maintaining the database consistency. The concurrent execution of a transaction is provided with atomicity, consistency, isolation, durability, and serializability via the concurrency control protocols.
- Locked based concurrency control protocol
- Timestamp based concurrency control protocol
### Locked based Protocol

In [locked based protocol](https://www.geeksforgeeks.org/lock-based-concurrency-control-protocol-in-dbms/), each transaction needs to acquire locks before they start accessing or modifying the data items. There are two types of locks used in databases.

- ****Shared Lock :**** Shared lock is also known as read lock which allows multiple transactions to read the data simultaneously. The transaction which is holding a shared lock can only read the data item but it can not modify the data item.
- ****Exclusive Lock :**** Exclusive lock is also known as the write lock. Exclusive lock allows a transaction to update a data item. Only one transaction can hold the exclusive lock on a data item at a time. While a transaction is holding an exclusive lock on a data item, no other transaction is allowed to acquire a shared/exclusive lock on the same data item.

There are two kind of lock based protocol mostly used in database:

- ****Two Phase Locking Protocol :****  [Two phase locking](https://www.geeksforgeeks.org/two-phase-locking-protocol/) is a widely used technique which ensures strict ordering of lock acquisition and release. Two phase locking protocol works in two phases.
    - ****Growing Phase :**** In this phase, the transaction starts acquiring locks before performing any modification on the data items. Once a transaction acquires a lock, that lock can not be released until the transaction reaches the end of the execution.
    - ****Shrinking Phase :**** In this phase, the transaction releases all the acquired locks once it performs all the modifications on the data item. Once the transaction starts releasing the locks, it can not acquire any locks further. 
- ****Strict Two Phase Locking  Protocol :**** It is almost similar to the two phase locking protocol the only difference is that in two phase locking the transaction can release its locks before it commits, but in case of strict two phase locking the transactions are only allowed to release the locks only when they performs commits. 

### ****Timestamp based Protocol****

- In this protocol each transaction has a [timestamp](https://www.geeksforgeeks.org/timestamp-based-concurrency-control/) attached to it. Timestamp is nothing but the time in which a transaction enters into the system.
- The conflicting pairs of operations can be resolved by the timestamp ordering protocol through the utilization of the timestamp values of the transactions. Therefore, guaranteeing that the transactions take place in the correct order.

## Advantages of Concurrency

In general, concurrency means, that more than one transaction can work on a system. The advantages of a concurrent system are:

- ****Waiting Time:**** It means if a process is in a ready state but still the process does not get the system to get execute is called waiting time. So, concurrency leads to less waiting time.
- ****Response Time:**** The time wasted in getting the response from the cpu for the first time, is called response time. So, concurrency leads to less Response Time.
- ****Resource Utilization:**** The amount of Resource utilization in a particular system is called Resource Utilization. Multiple transactions can run parallel in a system. So, concurrency leads to more Resource Utilization.
- ****Efficiency:**** The amount of output produced in comparison to given input is called efficiency. So, Concurrency leads to more Efficiency.

## Disadvantages of Concurrency 

- ****Overhead:**** Implementing concurrency control requires additional overhead, such as acquiring and releasing locks on database objects. This overhead can lead to slower performance and increased resource consumption, particularly in systems with high levels of concurrency.
- ****Deadlocks:**** Deadlocks can occur when two or more transactions are waiting for each other to release resources, causing a circular dependency that can prevent any of the transactions from completing. Deadlocks can be difficult to detect and resolve, and can result in reduced throughput and increased latency.
- ****Reduced concurrency:**** Concurrency control can limit the number of users or applications that can access the database simultaneously. This can lead to reduced concurrency and slower performance in systems with high levels of concurrency.
- ****Complexity:**** Implementing concurrency control can be complex, particularly in distributed systems or in systems with complex transactional logic. This complexity can lead to increased development and maintenance costs.
- ****Inconsistency:**** In some cases, concurrency control can lead to inconsistencies in the database. For example, a transaction that is rolled back may leave the database in an inconsistent state, or a long-running transaction may cause other transactions to wait for extended periods, leading to data staleness and reduced accuracy.