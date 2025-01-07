In PostgreSQL, you can create custom data types using two primary methods: **CREATE DOMAIN** and **CREATE TYPE**. Each method serves different purposes and offers unique capabilities.

## Creating Custom Data Types

### 1. Using CREATE DOMAIN

The **CREATE DOMAIN** command allows you to define a new data type that is based on an existing type but includes additional constraints such as `NOT NULL` or `CHECK`. This is useful for enforcing specific rules on the data that can be stored in that domain.

**Example: Creating a Domain**

```sql
CREATE DOMAIN phone_number AS VARCHAR(15)
    CHECK (VALUE ~ '^\+?[0-9]*$');
```

In this example:
- A new domain called `phone_number` is created based on the `VARCHAR(15)` type.
- It includes a check constraint to ensure that the value consists only of digits and an optional leading plus sign.

### 2. Using CREATE TYPE

The **CREATE TYPE** command is used to create composite types, which are essentially structures that can hold multiple fields, similar to a row in a table. This is particularly useful for returning multiple values from functions.

**Example: Creating a Composite Type**

```sql
CREATE TYPE employee_data AS (
    employee_name VARCHAR,
    contact_no VARCHAR
);
```

In this example:
- A new composite type called `employee_data` is created with two fields: `employee_name` and `contact_no`.
- This type can be used as the return type for functions or in table definitions.

## Using Custom Data Types

Once you have defined your custom data types, you can use them in various ways:

### Using Domain in Table Definition

```sql
CREATE TABLE contacts (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    phone phone_number
);
```

Here, the `phone` column uses the custom domain `phone_number`, ensuring all entries conform to the defined format.

### Using Composite Type in Functions

You can define a function that returns your custom composite type:

```sql
CREATE FUNCTION get_employee_data(emp_id INT) 
RETURNS employee_data AS $$
DECLARE
    result employee_data;
BEGIN
    SELECT employee_name, contact_no INTO result 
    FROM employee_info 
    WHERE id = emp_id;
    RETURN result;
END;
$$ LANGUAGE plpgsql;
```

In this function:
- It retrieves an employee's name and contact number based on their ID and returns it as an `employee_data` type.

## Conclusion

Creating custom data types in PostgreSQL enhances data integrity and allows for more complex data structures. By using **CREATE DOMAIN** for simple constraints and **CREATE TYPE** for composite types, you can tailor your database schema to better fit your application's needs.

Citations:
[1] https://www.sqlservercentral.com/articles/array-and-custom-data-types-in-postgresql
[2] https://neon.tech/postgresql/postgresql-tutorial/postgresql-user-defined-data-types
[3] https://www.commandprompt.com/education/how-to-create-a-user-defined-data-type-in-postgresql/
[4] https://www.javatpoint.com/postgresql-user-defined-data-type