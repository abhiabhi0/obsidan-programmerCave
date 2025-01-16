### Indexing Internals
- **Definition**: Indexing is a technique to improve data retrieval speed in databases by creating a separate data structure that maps search keys to their corresponding data locations.
### Types of Indexes
1. **Clustered Index**:
   - Data rows are stored in order based on the index key.
   - Only one clustered index per table.
   - Example: Primary key often implemented as a clustered index.
   
1. **Non-Clustered Index**:
   - Separate structure from data rows; contains pointers to the actual data.
   - Multiple non-clustered indexes can exist on a table.

2. **Unique Index**:
   - Ensures all values in the indexed column are distinct.

3. **Composite Index**:
   - Index on multiple columns to optimize queries filtering by those columns.

4. **Full-Text Index**:
   - Optimized for searching text within string columns.

### Internal Structures
- **B-Tree Structure**:
  - Most common structure for indexes.
  - Allows sorted data and efficient searching, insertion, and deletion.
  
  #### B-Tree Diagram
  ```
        [Root]
         / \
       /     \
    [A]      [B]
    / \      / \
  [C] [D] [E] [F]
  ```

- **Leaf Nodes**: Store actual data pointers.
- **Non-Leaf Nodes**: Store keys and pointers to child nodes.

### How Indexing Works
1. **Data Structure Creation**:
   - An index is created on specific columns, maintaining a sorted order of values.

2. **Search Optimization**:
   - Binary search or other efficient algorithms are used on the index to locate data quickly.

3. **Pointer References**:
   - Each entry in the index points to the actual row in the table, allowing quick access.

### Advantages of Indexing

- **Improved Query Performance**: Faster retrieval of rows matching specific values.
- **Efficient Data Access**: Reduces disk I/O operations by keeping frequently accessed data in memory.
- **Optimized Sorting Operations**: Avoids full table scans for sorting by using indexed columns.
- **Consistent Performance**: Maintains performance levels as data volume increases.

### Trade-offs and Considerations

- **Storage Overhead**: Additional disk space required for index structures.
- **Maintenance Costs**: Updates to indexed columns require index updates, adding overhead during write operations.
- **Choosing Right Indexes**: Requires analysis of query patterns to avoid over-indexing.

### Conclusion

Indexing is essential for enhancing database performance by providing efficient access paths to data. Understanding its internals—such as types, structures, and benefits—enables better database design and optimization strategies.

Citations:
[1] https://www.geeksforgeeks.org/indexing-in-databases-set-1/
[2] https://www.ibm.com/docs/en/watson-explorer/12.0.x?topic=indexing-internals
[3] https://www.freecodecamp.org/news/database-indexing-at-a-glance-bb50809d48bd/
[4] https://builtin.com/data-science/b-tree-index
[5] https://www.atlassian.com/data/databases/how-does-indexing-work
[6] https://www.sqlshack.com/sql-server-clustered-indexes-internals-with-examples/
[7] https://courses.cs.washington.edu/courses/cse444/17wi/sections/section2.pdf