A **unique constraint** in databases typically allows **NULL** values because NULL is treated as "unknown" or "undefined." The idea is that since NULL represents the absence of a value, two NULL values are not considered equal. This behavior can differ across database implementations, but here is a general explanation:

### **Behavior of NULL with Unique Constraints**

1. **Unique Constraint Logic:**
    
    - A unique constraint ensures that all non-NULL values in the column(s) must be unique.
    - NULL values are exempt from the uniqueness check because they do not represent an actual value.
2. **Database-Specific Behavior:**
    
    - **SQL Standard:** Allows multiple NULLs in a unique column.
    - **MySQL:** Allows multiple NULLs when a unique constraint is applied.
    - **PostgreSQL:** Allows multiple NULLs in unique constraints.
    - **Oracle:** Allows multiple NULLs in unique constraints.
    - **SQL Server:** Allows multiple NULLs in unique constraints.
3. **Why is this allowed?**
    
    - In SQL, comparisons involving NULL (e.g., `NULL = NULL`) evaluate to `UNKNOWN` rather than `TRUE`.
    - This means the database cannot definitively assert that one NULL equals another, so it does not consider them a violation of uniqueness.

### **Example**

**SQL Query:**

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(255) UNIQUE
);

INSERT INTO users (id, email) VALUES (1, 'test@example.com');
INSERT INTO users (id, email) VALUES (2, NULL);
INSERT INTO users (id, email) VALUES (3, NULL); -- Allowed, no uniqueness conflict
```

### **Contrast with Primary Constraints**

- **Primary constraints** do not allow NULLs because they are designed to uniquely identify every row in a table. If NULL were allowed, it would violate this principle.

### **Summary**

- Unique constraints focus on ensuring non-NULL values are unique.
- Primary constraints ensure every value (non-NULL and unique) is a valid identifier.

Would you like this included in your document, or do you want further clarification?