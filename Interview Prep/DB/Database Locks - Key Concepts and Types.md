### Overview

Database locks are mechanisms that manage concurrent access to data, ensuring data integrity and consistency. Locks prevent conflicting operations on the same resource.

---
### **Types of Locks**

1. **Shared Lock (S Lock):**
    - Allows multiple transactions to read a resource concurrently.
    - Prevents modifications by any transaction while the lock is held.
    - **SQL Example:**
        
        ```sql
        BEGIN TRANSACTION;
        SELECT * FROM table_name WITH (HOLDLOCK);
        -- Transaction holds a shared lock until committed or rolled back
        COMMIT;
        ```
        
2. **Exclusive Lock (X Lock):**
    - Grants a single transaction the ability to read and modify a resource.
    - Blocks other transactions from acquiring any type of lock on the resource.
    - **SQL Example:**
        
        ```sql
        BEGIN TRANSACTION;
        UPDATE table_name
        SET column_name = 'value'
        WHERE condition;
        -- Exclusive lock held on the row(s) being updated
        COMMIT;
        ```
        
3. **Update Lock (U Lock):**
    - Used when a transaction intends to update a resource.
    - Prevents deadlocks by ensuring only one transaction can transition to an exclusive lock.
    - **SQL Example:**
        
        ```sql
        BEGIN TRANSACTION;
        SELECT * FROM table_name WITH (UPDLOCK)
        WHERE condition;
        -- Update lock held to prevent deadlocks during update
        UPDATE table_name
        SET column_name = 'value'
        WHERE condition;
        COMMIT;
        ```
        
4. **Schema Lock:**
    - Protects database object structures, such as tables and indexes.
    - **SQL Example:**
        
        ```sql
        BEGIN TRANSACTION;
        ALTER TABLE table_name ADD column_name datatype;
        -- Schema modification lock held during the alteration
        COMMIT;
        ```
        
5. **Bulk Update Lock (BU Lock):**
    - Optimizes performance during bulk insert operations by reducing the number of locks required.
    - **SQL Example:**
        
        ```sql
        BULK INSERT table_name
        FROM 'file_path'
        WITH (TABLOCK);
        -- Bulk update lock used to improve performance
        ```
        
6. **Key-Range Lock:**
    - Used in indexed data to prevent phantom reads by locking a range of keys.
    - **SQL Example:**
        
        ```sql
        BEGIN TRANSACTION;
        SELECT * FROM table_name WITH (SERIALIZABLE)
        WHERE column_name BETWEEN value1 AND value2;
        -- Key-range lock applied to prevent phantom reads
        COMMIT;
        ```
        
7. **Row-Level Lock:**
    - Locks specific rows in a table, enabling other rows to be accessed concurrently.
    - **SQL Example:**
        
        ```sql
        BEGIN TRANSACTION;
        SELECT * FROM table_name WITH (ROWLOCK)
        WHERE condition;
        -- Row-level lock applied to the selected rows
        COMMIT;
        ```
        
8. **Page-Level Lock:**
    - Locks a specific page (a fixed-size block of data) within the database.
    - **SQL Example:**
        
        ```sql
        BEGIN TRANSACTION;
        SELECT * FROM table_name WITH (PAGLOCK)
        WHERE condition;
        -- Page-level lock applied to the data page
        COMMIT;
        ```
        
9. **Table-Level Lock:**
    - Locks an entire table. Simple to implement but reduces concurrency significantly.
    - **SQL Example:**
        
        ```sql
        BEGIN TRANSACTION;
        SELECT * FROM table_name WITH (TABLOCK);
        -- Entire table locked for the transaction
        COMMIT;
        ```

---


---
### **Summary of Key Locking Concepts**

- **Deadlocks:** Occur when transactions cyclically wait for each other’s locks. Mitigated using timeouts.
- **Phantom Reads:** Occur when new rows appear in query results due to concurrent inserts.
- **Lock Granularity:** Varies from row-level to table-level, balancing performance and concurrency.