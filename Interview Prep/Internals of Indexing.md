### Indexing Summary
- **Purpose**: Organizes an unordered table to improve query efficiency.
- **Definition**: A data structure (B-tree or its variants) that stores indexed column values and pointers to corresponding rows in the table's heap.
### Internal Representation
- **Example**: Table `employee` with `id` as a primary key automatically creates an index.
- **Heap Representation**: Table rows are stored in pages. Index points to exact row locations in these pages.
### Query Execution Scenarios
#### **With Index**
- **Process**:
    - Query optimizer chooses an **index scan**.
    - Searches through the index to find the `id`.
    - Uses the pointer to locate the corresponding row in the heap.
- **Efficiency**: Fast and optimal for queries on large datasets.
#### **Without Index**
- **Process**:
    - Query optimizer defaults to a **full table scan**.
    - Examines all rows in the table to find matches for the query condition.
    - May leverage **parallel scans** to improve performance.
- **Efficiency**: Acceptable for small datasets but slow for large tables.
### Query Optimization Factors

- **Data Size**: Determines whether an index or full table scan is better.
- **Indexes**: Presence and type of indexes influence scan choice.
- **Statistics**: Internal database stats guide the optimizer’s decision (e.g., row distribution, table fragmentation).
### Key Takeaway
Indexes significantly reduce search time for large datasets but come with overhead for maintenance and storage.

---
Indexing is the way to get an unordered table into an order that will maximize the query’s efficiency while searching.

An index is a data structure on disk & in-memory that stores a column value(onto which index is created) along with pointers to the corresponding rows of table in heap. This data structure is usually implemented as a B-tree (Balanced Tree) or it’s variation.

Suppose we have table of **employee**. Below are table representation, single page representation of records and heap representation to have better understanding.

![[Pasted image 20240614101549.png]]

Now let’s say it has ‘**id**’ as the primary key, and an index is automatically created for it. Below is the representation for the same. You can see index representation of primary key ‘**id**’ which is mapped to the exact location of rows in heap pages.

![[Pasted image 20240614101653.png]]

Lets see how this will work internally when we hit “**select name from Employee where ‘id=10**” with index ‘**id**’ and without index.

> “Each time a query is executed, the initial task undertaken by the database engine is **query optimisation**. This process involves evaluating the query, considering the table’s data size, taking into account any indexes established on the table and analysing some internal statistics tables. Based on these factors, the database engine determines whether to proceed with a full table scan, a bitmap scan, or an index scan. ”

## Scenario When index on ‘id’ exist

In this scenario, the database query optimiser will assess the query, table data and choose to execute an index scan due to the presence of an index on the ‘**id**’ field. This scan involves searching through the index, once the index is located, corresponding row’s location in the page is utilised to retrieve the ‘**name**’ field associated with it.

## Scenario When index does not exist

In this scenario, the database query optimiser will assess the most effective approach for handling this query. Given the absence of an index on the table, it will select a full table scan strategy. This strategy involves examining the entire table using the query’s specified value. While this approach is acceptable for smaller tables, it becomes inefficient for larger datasets. Mostly, database might internally optimize the process and conduct a parallel table scan using multiple threads.