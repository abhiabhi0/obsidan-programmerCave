## Types of Indexing

### 1. **Primary Index**
- **Definition**: A primary index is automatically created when a primary key constraint is defined on a table. It uniquely identifies each record in the table.
- **Characteristics**:
  - It determines the physical order of data storage.
  - There can only be one primary index per table.
  - It is typically implemented as a clustered index.

### 2. **Clustered Index**
- **Definition**: A clustered index sorts and stores the data rows in the table based on the values of the indexed column(s).
- **Characteristics**:
  - Only one clustered index can exist per table because it defines the physical order of data.
  - It is efficient for range queries and lookups.
  
### 3. **Non-Clustered Index (Secondary Index)**
- **Definition**: A non-clustered index creates a separate structure from the actual data, containing pointers to the data rows.
- **Characteristics**:
  - Multiple non-clustered indexes can exist on a single table.
  - It is useful for columns that are frequently queried but are not part of the primary key.

### 4. **Dense Index**
- **Definition**: In a dense index, an index entry is created for every search key value in the database table.
- **Characteristics**:
  - Provides fast access to records but requires more space due to additional entries.
  - Useful for unique keys.

### 5. **Sparse Index**
- **Definition**: A sparse index contains entries for only some of the search key values, typically pointing to blocks rather than individual records.
- **Characteristics**:
  - Requires less storage space compared to dense indexes.
  - Slower for locating records since it may require scanning multiple blocks.

### 6. **Bitmap Index**
- **Definition**: A bitmap index uses bit arrays (bitmaps) to represent the presence or absence of values in a column, particularly effective for low-cardinality columns (few distinct values).
- **Characteristics**:
  - Excellent for complex queries involving multiple conditions (e.g., AND, OR).
  - Commonly used in data warehousing applications.

### 7. **Hash Index**
- **Definition**: A hash index uses a hash function to map keys to specific locations in memory, allowing for very fast lookups for exact match queries.
- **Characteristics**:
  - Not suitable for range queries because it does not maintain order.
  - Typically used for primary keys or unique fields.

### 8. **Inverted Index**
- **Definition**: An inverted index maps content words to their locations in documents, enabling efficient full-text searches.
- **Characteristics**:
  - Commonly used in search engines and applications that require text searching capabilities.

### 9. **Composite Index**
- **Definition**: A composite index is created on multiple columns within a table, allowing queries that filter or sort by those columns to be executed more efficiently.
- **Characteristics**:
  - Useful when queries involve multiple columns in their WHERE clause.

### 10. **Filtered Index**
- **Definition**: A filtered index is created on a subset of rows based on a specified condition, improving performance by indexing only relevant rows.
- **Characteristics**:
  - Reduces overhead and improves query performance when dealing with large datasets where only a portion of the data is frequently accessed.

### 11. **Covering Index**
- **Definition**: A covering index includes all the columns needed by a query, allowing it to retrieve results directly from the index without accessing the underlying table data.
- **Characteristics**:
  - Enhances performance by reducing I/O operations since all required information is contained within the index.

### 12. **Function-Based Index**
- **Definition**: This type of index is created based on the result of a function or expression applied to one or more columns in a table.
- **Characteristics**:
  - Useful when queries frequently involve calculations or transformations on column values.

## Conclusion

Understanding these various types of indexes and their internal workings can significantly enhance your ability to optimize database performance. Each type of index serves different use cases and has its own advantages and disadvantages regarding speed, storage requirements, and suitability for specific query patterns. When designing your database schema, carefully consider which indexing strategies will best support your application's performance needs.

Citations:
[1] https://en.wikipedia.org/wiki/Database_index
[2] https://blog.algomaster.io/p/a-detailed-guide-on-database-indexes
[3] https://www.guru99.com/indexing-in-database.html
[4] https://www.geeksforgeeks.org/indexing-in-databases-set-1/
[5] https://www.javatpoint.com/indexing-in-dbms
[6] https://vertabelo.com/blog/database-index-types/
[7] https://www.linkedin.com/pulse/common-types-indexes-relational-databases-taras-sahaidachnyi-spljf