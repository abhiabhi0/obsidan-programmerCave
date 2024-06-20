- Indexes are lookup tables to speed up data retrieval in a database.
- They are references to data in a table, not viewable by users. 
- Indexes prevent duplicate entries in specified columns. 
- Indexes can be created using the CREATE INDEX command. 
- There are different types of indexes, such as single-column, unique, composite, and implicit indexes. 
- The DROP INDEX command is used to remove an index. 
```sql
CREATE INDEX name_of_Index ON name_Of_Table(Attribute1, Attribute2,...);

create INDEX ind_1 on studentdet(regno,dept);
```
- **Unique Indexes**: unique indexes are used for data integrity as well as accuracy. A unique index prevents duplicate values from being entered into the table.

```sql
CREATE UNIQUE INDEX name_index on tables_na (Attribute_name);
CREATE UNIQUE INDEX sidindex  on Student_DB (L_Name);
```
```sql
DROP INDEX name_of_index on Table_name;
```

### When Should Indexes Be Avoided
- On small tables
- Tables that receive a lot of big batch updates or inserts
- Columns that have large numbers of null values
- Columns that are frequently manipulated

### Advantages of Index in SQL
- Speed up select query
- Helps to make a row special or without duplicates (primary, unique)
- We can check against broad string values if the index is set to full-text index and find a word from a sentence.




