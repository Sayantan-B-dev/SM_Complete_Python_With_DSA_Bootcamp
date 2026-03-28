# SQL and SQLite: A Comprehensive Guide

This document provides an in‑depth exploration of SQL (Structured Query Language) and SQLite, their differences, and how to interact with SQLite databases using Python. It covers fundamental concepts, step‑by‑step operations, and realistic examples using sales data, with detailed explanations of each function, parameter, and best practice.

---

### 1.1. What is SQL?

**SQL (Structured Query Language)** is a standardized language for managing and manipulating relational databases. It allows users to:

- **Define** database schemas (tables, indexes, views)
- **Query** data using statements like `SELECT`
- **Manipulate** data (`INSERT`, `UPDATE`, `DELETE`)
- **Control** access permissions

SQL is declarative: you specify *what* you want, and the database engine determines *how* to retrieve it. It is used by virtually all relational database management systems (RDBMS) such as MySQL, PostgreSQL, Oracle, and SQLite.

---

### 1.2. What is SQLite?

**SQLite** is a self‑contained, serverless, zero‑configuration, transactional SQL database engine. Unlike client‑server databases, SQLite is embedded directly into the application. It reads and writes directly to ordinary disk files. Key characteristics:

- **No separate server process**: The entire database is a single file.
- **Cross‑platform**: Works on all major operating systems.
- **Zero configuration**: No setup, no administration.
- **Lightweight**: Ideal for mobile apps, desktop software, and testing.
- **ACID compliant**: Supports transactions, rollbacks, and concurrency.

SQLite is often used for prototyping, embedded systems, and applications where a full database server is overkill.

---

### 1.3. SQL vs. SQLite: Key Differences

| Feature              | SQL (as a language)                        | SQLite (as a database engine)                    |
|----------------------|--------------------------------------------|--------------------------------------------------|
| **Purpose**          | A language for querying relational data    | An actual database engine that implements SQL    |
| **Architecture**     | Not applicable                             | Serverless, embedded library                     |
| **Configuration**    | Not applicable                             | No configuration required                        |
| **Data types**       | Standard data types (INT, VARCHAR, etc.)   | Dynamic typing; affinity‑based type system       |
| **Concurrency**      | Varies by DBMS                             | Limited write concurrency (single writer)        |
| **Use case**         | Any relational database                    | Embedded, mobile, desktop, prototyping           |
| **Storage**          | Managed by the DBMS (files, network, etc.) | Single file on disk                              |

In practice, you write **SQL** statements that are executed by **SQLite** (or another database engine). The syntax is mostly standard, though each engine may have minor extensions.

---


## 2. Importing the sqlite3 Module

```python
import sqlite3
```

- The `sqlite3` module is part of Python’s standard library, so no additional installation is required.

---

## 3. Establishing a Connection

```python
connection = sqlite3.connect('example.db')
```

- `sqlite3.connect(database)` opens a connection to the SQLite database file.  
- If `'example.db'` does not exist, SQLite creates it automatically.  
- The return value is a `Connection` object that represents the database session.  
- You can use `:memory:` instead of a filename to create an in‑memory database (useful for testing).

---

## 4. Creating a Cursor

```python
cursor = connection.cursor()
```

- A **cursor** is an object that lets you execute SQL statements and fetch results.  
- Multiple cursors can be created from the same connection, but they share the same transaction context.

---

## 5. Creating a Table

```python
cursor.execute('''
    CREATE TABLE IF NOT EXISTS employees (
        id INTEGER PRIMARY KEY,
        name TEXT NOT NULL,
        age INTEGER,
        department TEXT
    )
''')
```

- `cursor.execute(sql)` sends the SQL statement to the database.  
- The triple‑quoted string allows multi‑line SQL for readability.  
- `CREATE TABLE IF NOT EXISTS` ensures the table is created only if it doesn’t already exist, avoiding an error.  
- **Columns:**
  - `id INTEGER PRIMARY KEY`: An auto‑incrementing integer primary key. SQLite automatically assigns a unique value.  
  - `name TEXT NOT NULL`: A text column that cannot be `NULL`.  
  - `age INTEGER`: An integer column that can be `NULL`.  
  - `department TEXT`: A text column.

---

## 6. Committing the Transaction

```python
connection.commit()
```

- SQLite operates inside a transaction. Changes made by `CREATE TABLE`, `INSERT`, `UPDATE`, `DELETE` are not permanent until you call `commit()`.  
- If the program terminates without committing, the changes are rolled back.  
- `commit()` writes all pending changes to the database file.

---

## 7. Inserting Data

### Single Row Insert

```python
cursor.execute('''
    INSERT INTO employees (name, age, department)
    VALUES (?, ?, ?)
''', ('Krish', 32, 'Data Scientist'))
```

- **Parameterized query:** The `?` placeholders are replaced by the values in the tuple. This prevents SQL injection and correctly handles escaping of quotes.  
- The second argument to `execute()` must be a tuple (or list) of values, even if there is only one value.  

### Multiple Inserts (Separate Executions)

```python
cursor.execute('''
    INSERT INTO employees (name, age, department)
    VALUES (?, ?, ?)
''', ('Bob', 25, 'Engineering'))

cursor.execute('''
    INSERT INTO employees (name, age, department)
    VALUES (?, ?, ?)
''', ('Charlie', 35, 'Finance'))
```

- Each `execute()` call sends a separate `INSERT` statement.  
- After all inserts, you must `commit()` to make them permanent.

---

## 8. Querying Data

```python
cursor.execute('SELECT * FROM employees')
```

- This selects all columns from the `employees` table.  
- The `execute()` method does not return the data; it prepares the cursor to fetch results.

### Fetching All Rows

```python
rows = cursor.fetchall()
```

- `fetchall()` returns a list of tuples, where each tuple represents a row.  
- If no rows match, it returns an empty list.

```python
for row in rows:
    print(row)
```

- Iterate through the list and print each row.

### Alternative: Fetch One Row at a Time

```python
row = cursor.fetchone()   # returns the next row or None
```

- `fetchone()` is useful when you expect a single row or want to process rows in a loop without loading all into memory.

---

## 9. Updating Data

```python
cursor.execute('''
    UPDATE employees
    SET age = ?
    WHERE name = ?
''', (34, 'Krish'))
```

- `UPDATE` modifies existing rows.  
- The `WHERE` clause specifies which rows to update. Without it, all rows would be updated.  
- Parameter placeholders ensure safe substitution.  
- **Commit after update:** `connection.commit()` makes the change permanent.

---

## 10. Deleting Data

```python
cursor.execute('''
    DELETE FROM employees
    WHERE name = ?
''', ('Bob',))
```

- `DELETE FROM` removes rows that match the condition.  
- The comma after `'Bob'` makes it a single‑element tuple, required for parameter substitution.  
- **Commit** to finalize the deletion.

---

## 11. Bulk Insertion with `executemany`

```python
sales_data = [
    ('2023-01-01', 'Product1', 100, 'North'),
    ('2023-01-02', 'Product2', 200, 'South'),
    ('2023-01-03', 'Product1', 150, 'East'),
    ('2023-01-04', 'Product3', 250, 'West'),
    ('2023-01-05', 'Product2', 300, 'North')
]

cursor.executemany('''
    INSERT INTO sales (date, product, sales, region)
    VALUES (?, ?, ?, ?)
''', sales_data)
```

- `executemany()` executes the same SQL statement for each element in the sequence (list of tuples).  
- It is much faster than calling `execute()` in a loop, especially for large datasets, because it reduces round‑trips to the database.  
- The SQL statement uses `?` placeholders, and each tuple provides the values for one insertion.

---

## 12. Closing the Connection

```python
connection.close()
```

- Always close the connection when you are done to release file handles and free resources.

---

## 13. Error Handling and Best Practices

### 13.1 Using `try...except...finally`

```python
import sqlite3

try:
    conn = sqlite3.connect('example.db')
    cursor = conn.cursor()
    # ... operations ...
    conn.commit()
except sqlite3.Error as e:
    print(f"An error occurred: {e}")
    conn.rollback()  # Undo any changes made during the failed transaction
finally:
    if conn:
        conn.close()
```

- `sqlite3.Error` is the base exception for all SQLite errors.  
- `rollback()` discards changes made since the last commit.  
- The `finally` block ensures the connection is closed even if an exception occurs.

### 13.2 Using a Context Manager

```python
with sqlite3.connect('example.db') as conn:
    cursor = conn.cursor()
    # ... operations ...
    # No explicit commit/close needed; the connection auto‑commits on success
```

- Starting from Python 3.8, `sqlite3` connections can be used as context managers.  
- At the end of the `with` block, if no exception occurred, `commit()` is called automatically. If an exception occurs, `rollback()` is called. The connection is then closed.  
- This is the recommended way to manage connections.

### 13.3 Always Use Parameterized Queries

**Never** do this:

```python
cursor.execute(f"SELECT * FROM employees WHERE name = '{name}'")
```

- This is vulnerable to SQL injection.  
- Always use `?` placeholders and pass values as a tuple.

---

## 14. Working with Sales Data: A Complete Example

Below is a fully commented script that creates a sales table, inserts data, and runs several queries.

```python
import sqlite3

# 1. Connect to the database (creates the file if it doesn't exist)
connection = sqlite3.connect('sales_data.db')

# 2. Create a cursor
cursor = connection.cursor()

# 3. Create a table
cursor.execute('''
    CREATE TABLE IF NOT EXISTS sales (
        id INTEGER PRIMARY KEY,   -- auto‑incrementing ID
        date TEXT NOT NULL,       -- sale date (stored as text, but can be compared as dates)
        product TEXT NOT NULL,    -- product name
        sales INTEGER,            -- amount sold
        region TEXT               -- region where sale occurred
    )
''')
# Commit the table creation
connection.commit()

# 4. Prepare data for bulk insertion
sales_data = [
    ('2023-01-01', 'Product1', 100, 'North'),
    ('2023-01-02', 'Product2', 200, 'South'),
    ('2023-01-03', 'Product1', 150, 'East'),
    ('2023-01-04', 'Product3', 250, 'West'),
    ('2023-01-05', 'Product2', 300, 'North')
]

# 5. Insert multiple rows using executemany
cursor.executemany('''
    INSERT INTO sales (date, product, sales, region)
    VALUES (?, ?, ?, ?)
''', sales_data)
connection.commit()  # Save the inserts

# 6. Query: total sales per product (descending order)
print("=== Total Sales per Product ===")
cursor.execute('''
    SELECT product, SUM(sales) as total_sales
    FROM sales
    GROUP BY product
    ORDER BY total_sales DESC
''')
rows = cursor.fetchall()
for row in rows:
    print(row)   # e.g., ('Product2', 500), ('Product1', 250), ('Product3', 250)

# 7. Query: average sales per region, with count
print("\n=== Average Sales per Region ===")
cursor.execute('''
    SELECT region, AVG(sales) as avg_sales, COUNT(*) as count
    FROM sales
    GROUP BY region
''')
for row in cursor.fetchall():
    print(row)   # e.g., ('North', 200.0, 2), ('South', 200.0, 1), ('East', 150.0, 1), ('West', 250.0, 1)

# 8. Update: increase sales for Product1 on 2023-01-01
cursor.execute('''
    UPDATE sales
    SET sales = sales + 20
    WHERE product = ? AND date = ?
''', ('Product1', '2023-01-01'))
connection.commit()
print("\nAfter update: Product1 on 2023-01-01 increased by 20.")

# 9. Delete: remove rows with sales less than 150
cursor.execute('''
    DELETE FROM sales
    WHERE sales < 150
''')
connection.commit()
print("Deleted rows with sales < 150.")

# 10. Verify the final data
print("\n=== Remaining Sales Data ===")
cursor.execute('SELECT * FROM sales')
for row in cursor.fetchall():
    print(row)

# 11. Close the connection
connection.close()
```

---

## 15. Summary of Key Functions and Methods

| Function / Method           | Description                                                                                 |
|-----------------------------|---------------------------------------------------------------------------------------------|
| `sqlite3.connect(filename)` | Opens a connection to the SQLite database file.                                             |
| `connection.cursor()`       | Creates a cursor object used to execute SQL statements.                                     |
| `cursor.execute(sql, params)` | Executes a single SQL statement with optional parameter substitution.                      |
| `cursor.executemany(sql, seq)` | Executes the same SQL statement for each set of parameters in the sequence.               |
| `cursor.fetchall()`         | Returns a list of all remaining rows from the last executed query.                          |
| `cursor.fetchone()`         | Returns the next row as a tuple, or `None` if no more rows.                                 |
| `cursor.fetchmany(size)`    | Returns up to `size` rows as a list.                                                        |
| `connection.commit()`       | Commits the current transaction, making all changes permanent.                              |
| `connection.rollback()`     | Rolls back the current transaction, discarding any changes since the last commit.           |
| `connection.close()`        | Closes the database connection.                                                             |

---

## 16. Conclusion

This guide has covered the essential operations for using SQLite in Python, from creating databases and tables to inserting, querying, updating, and deleting data. The code examples are heavily commented to explain the purpose of each line. By following best practices such as parameterized queries, proper error handling, and using context managers, you can build robust, secure database‑driven applications.