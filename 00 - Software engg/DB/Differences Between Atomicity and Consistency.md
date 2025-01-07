### Atomicity
- **Definition**: Atomicity ensures that a transaction is treated as a single unit of work. This means that either all operations within the transaction are completed successfully, or none are applied at all. If any part of the transaction fails, the entire transaction is rolled back to maintain data integrity.
- **Example**: In a banking system, if a user attempts to transfer money from one account to another, the transaction involves two operations: deducting the amount from one account and adding it to another. If the deduction succeeds but the addition fails, atomicity guarantees that the deduction will also be rolled back, leaving both accounts unchanged.

### Consistency
- **Definition**: Consistency refers to the state of the database before and after a transaction. A database must transition from one valid state to another valid state after a transaction is executed. While data may not be consistent during the execution of a transaction, it must be consistent once the transaction is complete.
- **Example**: Continuing with the banking example, if a user transfers funds from an account with sufficient balance, the database should reflect this change accurately. If the transaction violates any integrity constraints (e.g., resulting in a negative balance), consistency ensures that such transactions are not allowed.

## How SQL Queries Guarantee Atomicity

SQL databases implement atomicity through various mechanisms:

1. **Transaction Management**:
   - SQL provides commands like `BEGIN TRANSACTION`, `COMMIT`, and `ROLLBACK` to manage transactions explicitly.
   - A transaction begins with `BEGIN TRANSACTION`, and all subsequent operations are part of this transaction.
   - If all operations succeed, `COMMIT` is issued to save changes; if any operation fails, `ROLLBACK` reverts all changes made during the transaction.

2. **Logging**:
   - Most SQL databases use write-ahead logging (WAL) to ensure atomicity. Changes are first written to a log file before being applied to the database.
   - In case of a failure or crash, the database can use this log to recover to a consistent state by either replaying completed transactions or rolling back incomplete ones.

3. **Locking Mechanisms**:
   - SQL databases use locks to prevent multiple transactions from interfering with each other. This isolation helps maintain atomicity by ensuring that no other transactions can see intermediate states of ongoing transactions.

4. **Error Handling**:
   - SQL queries can include error handling mechanisms (e.g., TRY-CATCH blocks in T-SQL) that help manage exceptions during transaction execution, allowing for appropriate rollback actions when errors occur.

By leveraging these mechanisms, SQL databases ensure that transactions adhere to atomicity principles, thereby maintaining data integrity and reliability in multi-user environments.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/47674619/81839a89-c852-47fa-a71c-92ad53496ed5/paste.txt