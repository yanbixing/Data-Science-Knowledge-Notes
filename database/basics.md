# Basics

## Keys

### Different keys:

- Primary Key: A primary key is a a column or group of columns used to uniquely specify (identify) a record in a database table.
  - Primary key can be one or more column.
  - Multi-column primary key is also called "composite" primary key.
  - Primary key CANNOT be NULL.
  - Each table have at most 1 defined primary key
    - Others keys that can uniquely identify record are called unique/alternative key.
  - Ref: 
    - [GeeksForGeeks](https://www.geeksforgeeks.org/difference-between-primary-key-and-foreign-key/)
- Unique Key: A unique key is also a column or group of columns used to uniquely identify a record in a database table.
  - Unique key can be NULL. 
    - Seems the only difference, or we can use unique key as primary key.
  - Each table can have multiple unique keys, as long as we can identify a unique row by that key.
    - Unique keys are also called alternate keys.
  - Ref: [JavaPoint](https://www.javatpoint.com/unique-key-in-sql), [Wiki-Unique_key](https://en.wikipedia.org/wiki/Unique_key)
- Foreign Key: A foreign key is a column or group of columns that provides a link between data in two tables. 
  - It is a column (or columns) that references a column (most often the primary key) of another table.
  - Ref: 
    - [GeeksForGeeks](https://www.geeksforgeeks.org/difference-between-primary-key-and-foreign-key/)

### Comparison: 

- Primary Key
  - Primary key cannot have a NULL value.
  - Each table can have only one primary key.
  - By default, Primary key is clustered index, and the data in database table is physically organized in the sequence of clustered index.
  - Primary key can be related to another tables as a Foreign Key.
  - We can generate ID automatically with the help of Auto Increment field. Primary key supports Auto Increment value.
  - We can define Primary key constraint on temporary table and table variable.
  - We can't delete primary key value from the parent table which is used as a foreign key in child table. To delete we first need to delete that primary key value from the child table.
- Unique Key
  - Unique Constraint may have a NULL value.
  - Each table can have more than one Unique Constraint.
  - By default, Unique key is a unique non-clustered index.
  - Unique Constraint can not be related with another table's as a Foreign Key.
- Foreign Key
  - Foreign key is a field in the table that is Primary key in another table.
  - Foreign key can accept multiple null value.
  - Foreign key do not automatically create an index, clustered or non-clustered. You can manually create an index on foreign key.
  - We can have more than one foreign key in a table.
  - Foreign keys do not automatically create an index, clustered or non-clustered. You must manually create an index on foreign keys.
  - There are actual advantages to having a foreign key be supported with a clustered index, but you get only one per table. What's the advantage? If you are selecting the parent plus all child records, you want the child records next to each other. This is easy to accomplish using a clustered index.
  - Having a null foreign key is usually a bad idea instead of NULL  referred to as "orphan record".
  - We can’t define foreign key constraint on temporary table or table variable.
  - We can delete the foreign key value from the child table even though that refers to the primary key of the parent table.

Ref: [CsharpCorner-blog](https://www.c-sharpcorner.com/blogs/difference-between-primary-key-unique-key-and-foreign-key1)