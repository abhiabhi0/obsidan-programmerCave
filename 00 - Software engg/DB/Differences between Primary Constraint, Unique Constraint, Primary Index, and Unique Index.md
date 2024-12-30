### **Primary Constraint vs. Unique Constraint**

#### **Primary Constraint:**

- **Purpose:** Uniquely identifies each record in a table.
- **Characteristics:**
    - Enforces uniqueness on the column(s).
    - Does **not allow NULL** values in any column of the constraint.
    - A table can have **only one primary constraint**.
- **Usage:** Typically used for the main identifier of a table, like `id`.
- **SQL Example:**
    
    ```sql
    CREATE TABLE employees (
        id INT PRIMARY KEY,
        name VARCHAR(100)
    );
    ```
    

#### **Unique Constraint:**

- **Purpose:** Ensures that all values in the specified column(s) are unique.
- **Characteristics:**
    - Enforces uniqueness on the column(s).
    - Allows **NULL values**, but the behavior can vary (e.g., some databases allow multiple NULLs in a unique column).
    - A table can have **multiple unique constraints**.
- **Usage:** Used for non-primary fields that still require uniqueness, like an email address.
- **SQL Example:**
    
    ```sql
    CREATE TABLE users (
        id INT PRIMARY KEY,
        email VARCHAR(255) UNIQUE
    );
    ```
    
[[Unqiue constraint allows NULL]]

---

### **Primary Index vs. Unique Index**

#### **Primary Index:**

- **Purpose:** Automatically created when a primary constraint is defined. It optimizes query performance for the primary key.
- **Characteristics:**
    - Ensures all primary key values are unique.
    - Clustered by default in some databases (e.g., MySQL InnoDB), meaning table data is physically stored in order of the primary key.
    - A table can have only one primary index.
- **SQL Example:** Automatically created with the primary key constraint.

#### **Unique Index:**

- **Purpose:** Explicitly created to enforce uniqueness or improve query performance on non-primary key columns.
- **Characteristics:**
    - Ensures all indexed values are unique.
    - Non-clustered by default but can be clustered if specified.
    - Multiple unique indexes can exist on a table.
- **Usage:** Improves lookup speed on columns like `email` or `phone_number`.
- **SQL Example:**
    
    ```sql
    CREATE UNIQUE INDEX idx_unique_email
    ON users (email);
    ```
    

---

### **Key Differences at a Glance**

|Aspect|Primary Constraint|Unique Constraint|Primary Index|Unique Index|
|---|---|---|---|---|
|Enforces Uniqueness|Yes|Yes|Yes|Yes|
|Allows NULL Values|No|Yes (varies by DB)|No (in primary key)|Yes (varies by DB)|
|Automatic Creation|Yes (creates index)|No|Yes (for primary key)|No (manual creation)|
|Multiple in a Table|No|Yes|No|Yes|
|Clustered Index|Default in some DBs|No|Yes (in clustered DBs)|Optional|

Would you like to include detailed examples or expand on specific database behaviors?