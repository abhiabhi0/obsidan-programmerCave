- PostgreSQL is a full-featured object-relational database management system (ORDBMS)
- powerful cross-platform open source ORDBMS that runs on many operating systems
- PostgreSQL's main benefit is it's high data consistency and integrity, but it also encapsulates four important features: Atomicity, Consistency, Isolation, Durability (known as ACID), which ensure the transactions remain correct and reliable in DBMS during the process of writing or updating data.

### Transaction
A transaction is a sequence of one or more operations. For a database, this sequence of operations is either all executed or none executed, and is an inseparable unit of work for the database.

### Atomicity
Transactions usually consist of multiple statements. Atomicity guarantees that each transaction is treated as a single unit, and that the transaction either succeeds completely or fails completely.

An atomic system must guarantee atomicity under all circumstances, including power failures, database errors, and instance runs. At the same time, atomicity prevents the occurrence of partial data updates in the database.

### Consistency
Consistency ensures that transactions can only change the database from one valid state to another, guarantees that the final state of the database data is consistent.

### Isolation
Isolation guarantees that multiple transactions can occur simultaneously. The simplest example is that multiple transactions can read and write to a table at the same time. Isolation ensures that multiple transactions can proceed simultaneously and not affect each other.

### Durability
Durability is to ensure that once a transaction is committed, even in the event of a system exception (e.g., power failure, instance crash), the incomplete transaction can be rolled back, and the completed transaction can be automatically committed and recorded on a permanent storage medium.

### ACID in PostgreSQL

PostgreSQL mainly uses two technologies, Multi-Version Concurrency Control (MVCC) and Write-Ahead Logging (WAL), to implement ACID features, ensuring the validity of the data in case of exceptions.

1. In PostgreSQL, the transaction ID is left on the data operated by add, delete and change, so that the batch operation can be easily committed or completely undone, thus ensuring the atomicity of the transaction.
2. Using MVCC, read operations do not block write operations, and write operations do not block read operations, improving performance under concurrent access.
3. The rollback of a transaction can be done immediately, regardless of the number of operations performed by the transaction.
4. Data can be updated in large quantities and will not need to ensure that the rollback segment is not exhausted as in MySQL, InnoDB and Oracle.

## **Benefits of PostgreSQL**

### Stable

The multi-process architecture makes PostgreSQL more stable under abnormal conditions.

1. A subprocess will not affect the operation of other processes even if it crashes.
2. Under high concurrent reads and writes, PostgreSQL's performance metrics can maintain a hyperbolic or even logarithmic curve with no drop after the peak, while MySQL will clearly dip after the wave.

### Scalability

The scalability of a database system is directly dependent on the ability to compress data. Ideally, database systems must have advanced, off-the-shelf compression techniques. In some database systems, developers have to compress manually, which is not only time-consuming but also inefficient. PostgreSQL provides it for free and the whole process is automatic.

### Powerful Function

1. In terms of tables and views, PostgreSQL supports temporary tables and can use PL/pgSQL (Procedural Language/ Postgres SQL), PL/Perl, PL/Python to materialize views.
2. For indexes, it fully supports R-/R+tree indexes, full-text search, bitmap indexes, hash indexes, reverse indexes, partial indexes, expression indexes, Generalized Search Trees (GiST).
3. For other objects, PostgreSQL supports data fields, stored procedures, triggers, functions, external calls, and 
4. In terms of data table partitioning, it supports 4 kinds of partitioning, range partitioning, hash partitioning, hybrid partitioning, list partitioning.
5. In terms of stored procedures, PostgreSQL supports stored procedures. This point can avoid the transmission of a large number of raw SQL statements on the network.
6. In terms of extensions of user-defined functions, PostgreSQL can be more easily extended using User Defined Functions (UDF).
7. Point-In-Time-Recover (PITR), a feature introduced in PostgreSQL starting from version 8.0, allows database clusters to be recovered to any point in time using base backups and continuous archived logs.
8. Provide high availability services through asynchronous or synchronous replication methods across servers.

### Comparison of PostgreSQL and MySQL

**Open source**

- PostgreSQL is a free and open source system.
- MySQL is an open source system developed by Oracle and offers several paid versions for users to access.

**Performance**

- PostgreSQL is suitable for use in large-scale systems that require high read and write speeds.
- MySQL is mainly used for web applications which only need a database to perform data transactions.

**ACID compliance**

- PostgreSQL fully adheres to ACID principles and ensures all requirements are met.
- MySQL is only ACID-compliant when using InnoDB and the NDB cluster storage engine.

**Platform Support**

- PostgreSQL can run on Linux, Windows (Win2000 SP4 and above), FreeBSD, OpenBSD, NetBSD, Mac OS X, AIX, IRIX, Solaris, Tu64, HP-UX OS, and the open source Unix OS.
- MySQL can run on Oracle Solaris, Microsoft Windows, Linux Mac OS X, and open source FreeBSD OS.

**Programming Language Support**

- PostgreSQL is written in C. It supports several programming languages, C, C++, Delphi, JavaScript, Java, Python, R, Tcl (tool command language), Golang, Lisp, Erlang and .Net .
- MySQL is written in C and C++. It supports C/C++, Erlang, PHP, Lisp (ListProcessing), Golang, Perl, Java, Delphi, R, and Node.js.

**Materialized Views**

- PostgreSQL supports materialized views.
- MySQL does not support materialized views.

**Data Backup**

- PostgreSQL supports master-standby replication and can also handle other types of replication by implementing third-party extensions.
- MySQL supports master-standby replication, where each node is the master and has the right to update data.

**Scalability**

- PostgreSQL is highly scalable, can add and have data types, operators, index types and functional languages.
- MySQL does not support Scalability.

**Community Support**

- PostgreSQL has an active community of support that helps improve existing features, and its creative committers do their best to ensure that the database stays up-to-date with the latest features and maximum security, making it the most advanced database available.
- MySQL also has a large community of followers, while these community contributors, especially after its acquisition by Oracle, will only occasionally focus on new features as they emerge and maintain existing features.