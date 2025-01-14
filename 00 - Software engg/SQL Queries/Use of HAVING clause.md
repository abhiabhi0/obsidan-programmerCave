The **HAVING** clause in SQL is used to filter records that have been grouped by the **GROUP BY** clause based on specified conditions, particularly when dealing with aggregate functions. Here’s a detailed explanation of its purpose and usage:

### Purpose of the HAVING Clause

1. **Filtering Aggregated Data**: The HAVING clause allows you to apply conditions to groups created by the GROUP BY clause. This is essential when you want to filter results based on aggregate values, such as totals or averages, which cannot be done using the WHERE clause.

2. **Used with Aggregate Functions**: Unlike the WHERE clause, which filters rows before any grouping occurs, HAVING works on aggregated data and can include aggregate functions like COUNT(), SUM(), AVG(), MIN(), and MAX().

### Syntax

The basic syntax for using the HAVING clause is:

```sql
SELECT column1, aggregate_function(column2)
FROM table_name
WHERE condition
GROUP BY column1
HAVING condition;
```

### Key Points

- **Execution Order**: The HAVING clause is executed after the GROUP BY clause and before any ORDER BY clause.
- **Conditions**: You can specify one or more conditions using logical operators (AND, OR) in the HAVING clause.
- **Usage Context**: The HAVING clause is typically used in reports or queries where you need to summarize data and apply filters based on those summaries.

### Example Usage

Here are a few examples illustrating how to use the HAVING clause:

1. **Using COUNT()**:
   ```sql
   SELECT department, COUNT(*) AS employee_count
   FROM employees
   GROUP BY department
   HAVING COUNT(*) > 10;
   ```
   This query retrieves departments with more than 10 employees.

2. **Using SUM()**:
   ```sql
   SELECT department, SUM(salary) AS total_salary
   FROM employees
   GROUP BY department
   HAVING SUM(salary) > 50000;
   ```
   This example finds departments where the total salary exceeds 50,000.

3. **Using AVG()**:
   ```sql
   SELECT department, AVG(salary) AS average_salary
   FROM employees
   GROUP BY department
   HAVING AVG(salary) < 6000;
   ```
   Here, it retrieves departments with an average salary less than 6,000.

### Differences Between WHERE and HAVING

| Feature               | WHERE Clause                          | HAVING Clause                          |
|-----------------------|---------------------------------------|----------------------------------------|
| Filtering Stage       | Before grouping                       | After grouping                         |
| Usage with Aggregates | Cannot use aggregate functions        | Can use aggregate functions            |
| Purpose               | Filters individual records            | Filters groups of records              |

### Conclusion

The HAVING clause is a powerful tool in SQL for filtering grouped data based on aggregate results. It enhances your ability to analyze and report on data effectively by allowing conditions that are not possible with the WHERE clause alone. Understanding how to use it correctly will significantly improve your SQL query capabilities.

Citations:
[1] https://www.geeksforgeeks.org/sql-having-clause-with-examples/
[2] https://www.simplilearn.com/tutorials/sql-tutorial/sql-having
[3] https://www.javatpoint.com/having-clause-in-sql
[4] https://www.tutorialspoint.com/sql/sql-having-clause.htm
[5] https://www.shiksha.com/online-courses/articles/how-to-use-having-clause-in-sql/
[6] https://www.ibm.com/docs/en/informix-servers/12.10?topic=clauses-having-clause