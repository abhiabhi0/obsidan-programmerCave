### Key Techniques for SQL Query Optimization

1. **Use Indexes Effectively**
   - **Purpose**: Indexes help the database locate specific rows quickly without scanning the entire table.
   - **Implementation**: Identify columns commonly used in `WHERE`, `JOIN`, and `ORDER BY` clauses, and create indexes on them. However, avoid excessive indexing as it can slow down write operations.

2. **Avoid SELECT * and Retrieve Only Necessary Columns**
   - **Purpose**: Reducing the amount of data retrieved minimizes resource usage.
   - **Implementation**: Instead of `SELECT *`, specify only the columns you need, e.g., `SELECT column1, column2 FROM table_name`.

3. **Optimize JOIN Operations**
   - **Purpose**: Efficient joins can significantly improve query performance.
   - **Implementation**: Use appropriate join types (INNER, LEFT, etc.) and ensure that joined columns are indexed. Avoid unnecessary joins by filtering data early.

4. **Minimize the Use of Subqueries**
   - **Purpose**: Subqueries can be less efficient than joins.
   - **Implementation**: Rewrite subqueries as joins where possible to enhance performance.

5. **Use WHERE Clause Instead of HAVING**
   - **Purpose**: The `WHERE` clause filters records before grouping, while `HAVING` filters after.
   - **Implementation**: Use `WHERE` for filtering non-aggregated data to improve efficiency.

6. **Limit Result Set with LIMIT and OFFSET**
   - **Purpose**: Reduces the number of rows returned in a query, which is useful for pagination.
   - **Implementation**: Use `LIMIT n` to restrict results to the first n rows.

7. **Utilize EXISTS Instead of COUNT**
   - **Purpose**: `EXISTS` stops processing once it finds a match, whereas `COUNT` continues to count all occurrences.
   - **Implementation**: Use `SELECT * FROM table WHERE EXISTS (SELECT 1 FROM other_table WHERE condition)` instead of counting rows.

8. **Avoid Functions in Filtering Conditions**
   - **Purpose**: Applying functions to columns in the `WHERE` clause can prevent index usage.
   - **Implementation**: Write queries that allow the database to use indexes effectively without applying functions on indexed columns.

9. **Monitor Query Performance**
   - Use tools and techniques like query profiling to analyze execution times and identify slow queries for optimization.

10. **Consider Partitioning Large Tables**
    - **Purpose**: Partitioning can improve performance by dividing large tables into smaller, more manageable pieces.
    - **Implementation**: Implement partitioning based on key attributes like date or region.

### Example Optimization

#### Original Query
```sql
SELECT * FROM orders WHERE YEAR(order_date) = 2024;
```

#### Optimized Query
```sql
SELECT * FROM orders WHERE order_date >= '2024-01-01' AND order_date < '2025-01-01';
```
- The optimized query avoids using a function (`YEAR()`) in the filtering condition, which allows for better index utilization.

### Diagram Example

Here’s a simple diagram illustrating how indexes work:

```plaintext
+--------------------+
|      Table         |
|--------------------|
| ID | Name | Age    |
|----|------|--------|
| 1  | John | 30     |
| 2  | Jane | 25     |
| 3  | Mike | 35     |
+--------------------+
         |
         v
+--------------------+
|      Index         |
|--------------------|
| Age (Index)       |
|--------------------|
| 25 -> Row 2       |
| 30 -> Row 1       |
| 35 -> Row 3       |
+--------------------+
```
This diagram shows how an index on the "Age" column allows quick access to rows based on age values.

By implementing these techniques and strategies, you can significantly enhance the performance of your SQL queries, leading to faster response times and improved efficiency in database operations.

Citations:
[1] https://www.geeksforgeeks.org/best-practices-for-sql-query-optimizations/
[2] https://www.thoughtspot.com/data-trends/data-modeling/optimizing-sql-queries
[3] https://www.developernation.net/blog/12-ways-to-optimize-sql-queries-in-database-management/
[4] https://aiven.io/developer/sql-query-optimization-guide