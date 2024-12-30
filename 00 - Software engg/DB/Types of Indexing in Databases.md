

Indexing is a technique used to optimize the performance of a database by minimizing the number of disk accesses required for a query. Here are the main types of indexing:

---

### 1. **Primary Index**

- Built on the primary key of a table.
- Ensures that rows are stored in sorted order based on the primary key.
- There is one primary index per table.

**Query:**

```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50)
);
```

---

### 2. **Unique Index**

- Prevents duplicate values in the indexed column(s).
- Enforces the uniqueness constraint.

**Query:**

```sql
CREATE UNIQUE INDEX idx_unique_email
ON employees (email);
```

---

### 3. **Clustered Index**

- Sorts and stores the rows of a table based on the key values.
- There can only be one clustered index per table because the data rows are stored in the order of the clustered index.

**Query:**

```sql
CREATE CLUSTERED INDEX idx_clustered_empid
ON employees (emp_id);
```

---

### 4. **Non-Clustered Index**

- Creates a separate structure that holds the key value and a pointer to the actual row in the table.
- A table can have multiple non-clustered indexes.

**Query:**

```sql
CREATE NONCLUSTERED INDEX idx_nonclustered_name
ON employees (name);
```

---

### 5. **Composite Index**

- Indexes multiple columns together.
- Useful for queries involving multiple columns in `WHERE` or `JOIN` clauses.

**Query:**

```sql
CREATE INDEX idx_composite_name_dept
ON employees (name, department);
```

---

### 6. **Full-Text Index**

- Used for efficient text searches in large textual data.
- Supports complex queries like searching for words, phrases, and their proximity.

**Query:**

```sql
CREATE FULLTEXT INDEX ON employees (name, department)
KEY INDEX idx_unique_empid;
```

---

### 7. **Bitmap Index**

- Represents data in a bitmap structure.
- Used in databases like Oracle for low-cardinality columns (columns with a few unique values).

**Query (Oracle):**

```sql
CREATE BITMAP INDEX idx_bitmap_dept
ON employees (department);
```

---

### 8. **Hash Index**

- Used in NoSQL databases like MongoDB or Postgres for hash-based lookups.
- Useful for equality-based searches.

**Query (Postgres Example):**

```sql
CREATE INDEX idx_hash_email
ON employees USING HASH (email);
```

---

### Summary Table of Index Types

|**Type**|**Purpose**|**Example Column**|
|---|---|---|
|Primary Index|Uniquely identifies rows, sorted storage|Primary Key (ID)|
|Unique Index|Prevents duplicate values|Email Address|
|Clustered Index|Sorts and stores table rows|Primary Key (ID)|
|Non-Clustered Index|Creates a pointer-based index|Name, Department|
|Composite Index|Combines multiple columns|Name + Department|
|Full-Text Index|Optimized for text-based searches|Articles, Comments|
|Bitmap Index|Efficient for low-cardinality columns|Gender, Status|
|Hash Index|Fast lookups using hash|Email, Username|

Indexes enhance query performance but may add overhead during `INSERT`, `UPDATE`, and `DELETE` operations due to maintenance.