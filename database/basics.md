# Basics

## 1.Keys of table

### 2. Different keys:

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
    - Ref: [GeeksForGeeks](https://www.geeksforgeeks.org/difference-between-primary-key-and-foreign-key/)
  - It acts as a cross-reference between tables because it **references the primary key of another table**, thereby establishing a link between them.
    - Ref: [Techopedia](https://www.techopedia.com/definition/7272/foreign-key)

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
  - Foreign key is a field in the table that is **Primary key in another table.**
  - Foreign key can accept multiple null value.
  - Foreign key do not automatically create an index, clustered or non-clustered. You can manually create an index on foreign key.
  - We can have more than one foreign key in a table.
  - Foreign keys do not automatically create an index, clustered or non-clustered. You must manually create an index on foreign keys.
  - There are actual advantages to having a foreign key be supported with a clustered index, but you get only one per table. What's the advantage? If you are selecting the parent plus all child records, you want the child records next to each other. This is easy to accomplish using a clustered index.
  - Having a null foreign key is usually a bad idea instead of NULL  referred to as "orphan record".
  - We can’t define foreign key constraint on temporary table or table variable.
  - We can delete the foreign key value from the child table even though that refers to the primary key of the parent table.

Ref: [CsharpCorner-blog](https://www.c-sharpcorner.com/blogs/difference-between-primary-key-unique-key-and-foreign-key1)

[1p3a](https://www.1point3acres.com/bbs/thread-820480-1-1.html): Interview of L - what is foreign key

## 2. Join operations

### 2.1. Map-Side Join vs Reduce-Side Join

[1p3a](https://www.1point3acres.com/bbs/thread-820480-1-1.html): Interview of L - diff between map join vs reduce join

## 3. Table vs view

### 3.1. Table vs view

- Table
  - A table consists of rows and columns used to organize data 


- View: The view is a virtual/logical table formed as a result of a query and used to view or manipulate parts of the table. We can create the columns of the view from one or more tables. Its content is based on base tables.
  - Views are usually virtual and do not occupy space in systems.
  - Views enable us to hide some of the columns from the table.
  - It simplifies complex queries because it can draw data from multiple tables and present it as a single table.
  - It helps in data security that shows only authorized information to the users.
  - It presents a consistent, unchanged image of the database structure, even if the source tables are renamed, split, or restructured.

- Table vs View:
  - A table is a database object that holds information used in applications and reports. 
    - On the other hand, a view is also a database object utilized as a table and can also link to other tables.
  - A table consists of rows and columns to store and organized data in a structured format
    - while the **view is a result set of SQL statements**.
  - A table is structured with columns and rows, while a view is a virtual table extracted from a database.
  - The table is an independent data object while views are usually depending on the table.
  - The table is an actual or real table that exists in physical locations. On the other hand, views are the virtual or logical table that does not exist in any physical location.
  - A table allows to performs add, update or delete operations on the stored data. On the other hand, we cannot perform add, update, or delete operations on any data from a view. If we want to make any changes in a view, we need to update the data in the source tables.
  - We cannot replace the **table object** directly because it is **stored as a physical entry**. In contrast, we can easily use the replace option to recreate the view because it is a pseudo name to the SQL statement running behind on the database server.


- Personal summary:
  - Table: a physical stored data
  - View: result of query, not physically stored
    - More convenient to use, can be used as table, just not physically stored.
    - Can hid information, more secure

[1p3a](https://www.1point3acres.com/bbs/thread-818409-1-1.html): Interview of M - Table vs View





