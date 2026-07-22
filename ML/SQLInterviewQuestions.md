# SQL Questions and Answers by Mohamamd Sefidgar

## Section 1: General SQL Questions

### 1. What is SQL?
* **Summary:** SQL (Structured Query Language) is the standard language used to communicate with, manage, and manipulate relational databases.
* **Code:**
```sql
SELECT * FROM users;
```
* **Terms Used:**
  * `Relational Database`: A database that stores data in structured tables with rows and columns.
  * `Query`: A command sent to a database to perform actions like fetching or updating data.

---

### 2. What are SQL dialects? Give some examples.
* **Summary:** Dialects are different versions or extensions of standard SQL developed by different database vendors (e.g., PostgreSQL, MySQL, SQL Server, Oracle, SQLite).
* **Code:**
```sql
-- PostgreSQL specific string concatenation
SELECT 'Hello ' || 'World';

-- MySQL specific string concatenation
SELECT CONCAT('Hello ', 'World');
```
* **Terms Used:**
  * `SQL Dialect`: A database-specific implementation of the core ANSI SQL standard.
  * `Syntax`: The set of rules defining correct language structure.

---

### 3. What are the main applications of SQL?
* **Summary:** SQL is used to create and modify database schemas, read data, update existing records, delete unwanted records, and manage access permissions.
* **Code:**
```sql
INSERT INTO clients (id, name) VALUES (1, 'Alice');
UPDATE clients SET name = 'Bob' WHERE id = 1;
```
* **Terms Used:**
  * `CRUD`: Stands for Create, Read, Update, Delete — the four basic operations for persistent storage.
  * `Schema`: The organizational blueprint or structure of a database.

---

## Section 2: SQL Questions for Beginners

### 4. What is an SQL statement?
* **Summary:** An SQL statement is a valid instruction/command written in SQL that the database engine can parse and execute.
* **Code:**
```sql
DROP TABLE IF EXISTS old_logs;
```
* **Terms Used:**
  * `Database Engine`: The underlying software component that processes database operations.
  * `Statement`: A complete executable line or block of database code.

---

### 5. What is an SQL query?
* **Summary:** A query is a specific SQL statement written to retrieve, aggregate, or modify data within database tables.
* **Code:**
```sql
SELECT name, age FROM students WHERE age > 18;
```
* **Terms Used:**
  * `Filtering`: Restricting the returned records based on specific logical criteria.
  * `Result Set`: The tabular data returned by a SELECT query.

---

### 6. What is an SQL subquery?
* **Summary:** A subquery (or inner query) is a query embedded inside another SQL query to evaluate intermediate values.
* **Code:**
```sql
SELECT name FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees);
```
* **Terms Used:**
  * `Inner Query`: The nested subquery that runs first.
  * `Outer Query`: The main query that receives and processes the result of the inner query.

---

### 7. What is an SQL join?
* **Summary:** A join is an operation used to combine rows from two or more tables based on a common related column between them.
* **Code:**
```sql
SELECT orders.id, customers.name 
FROM orders 
JOIN customers ON orders.customer_id = customers.id;
```
* **Terms Used:**
  * `JOIN`: Clause connecting two tables on matching key columns.
  * `Key Column`: A column containing shared identifiers across tables.

---

### 8. What is an SQL comment?
* **Summary:** Comments are notes added into SQL code to explain logic. They are ignored by the query executor.
* **Code:**
```sql
-- Single line comment
/* Multi-line 
   comment */
SELECT * FROM products;
```
* **Terms Used:**
  * `Single-line comment`: Denoted by `--`.
  * `Multi-line comment`: Enclosed between `/*` and `*/`.

---

### 9. What is an SQL alias?
* **Summary:** An alias is a temporary name assigned to a table or column to make the query cleaner and easier to read.
* **Code:**
```sql
SELECT e.first_name AS name, e.salary * 12 AS annual_salary 
FROM employees AS e;
```
* **Terms Used:**
  * `AS`: The keyword used to assign a temporary alias name.

---

## Section 3: Technical SQL Questions

### 10. What types of SQL commands do you know?
* **Summary:** SQL commands fall into 5 main categories: DDL (structure), DML (data manipulation), DQL (querying), DCL (permissions), and TCL (transactions).
* **Code:**
```sql
-- DDL: CREATE, DML: INSERT, DQL: SELECT, DCL: GRANT, TCL: COMMIT
```
* **Terms Used:**
  * `DDL`: Data Definition Language.
  * `DML`: Data Manipulation Language.
  * `DQL`: Data Query Language.
  * `DCL`: Data Control Language.
  * `TCL`: Transaction Control Language.

---

### 11. Give some examples of common SQL commands.
* **Summary:** Basic commands include `CREATE`, `ALTER`, `DROP` (DDL); `INSERT`, `UPDATE`, `DELETE` (DML); `SELECT` (DQL); `GRANT`, `REVOKE` (DCL); `COMMIT`, `ROLLBACK` (TCL).
* **Code:**
```sql
CREATE TABLE test (id INT);
INSERT INTO test VALUES (1);
SELECT * FROM test;
```
* **Terms Used:**
  * `DDL / DML / DQL`: Categories governing database objects, data modification, and querying.

---

### 12. What is DBMS, and what types of DBMS do you know?
* **Summary:** A Data Base Management System is software used to store, retrieve, and manage data. Types include Relational (RDBMS), NoSQL (Document, Key-Value), Object-Oriented, and Hierarchical.
* **Code:**
```sql
-- SQL DBMS handles structured relational data like this:
SELECT * FROM users;
```
* **Terms Used:**
  * `DBMS`: Database Management System.
  * `NoSQL`: Non-relational databases optimized for unstructured or semi-structured data.

---

### 13. What is RDBMS? Give some examples of RDBMS.
* **Summary:** A Relational DBMS organizes data into structured tables linked by relationships (keys). Examples: PostgreSQL, MySQL, Oracle, SQL Server, SQLite.
* **Code:**
```sql
-- Structured relational table access
SELECT employees.name, departments.dept_name 
FROM employees 
JOIN departments ON employees.dept_id = departments.id;
```
* **Terms Used:**
  * `Relational Model`: Structuring data into tabular rows and columns with linked keys.

---

### 14. What are tables and fields in SQL?
* **Summary:** A table is a structured collection of related data composed of rows (records) and columns (fields/attributes).
* **Code:**
```sql
-- Column 'email' is a field; 'users' is the table
CREATE TABLE users (
    id INT,           -- Field
    email VARCHAR(50) -- Field
);
```
* **Terms Used:**
  * `Field / Column`: An attribute holding specific type data.
  * `Record / Row`: A single entry representing one entity instance.

---

### 15. What types of SQL subqueries do you know?
* **Summary:** Subqueries can be Single-row (returns 1 value), Multi-row (returns multiple values), Multi-column, Nested, or Correlated (depends on outer query).
* **Code:**
```sql
-- Single-row subquery
SELECT * FROM sales WHERE amount = (SELECT MAX(amount) FROM sales);

-- Multi-row subquery
SELECT * FROM sales WHERE customer_id IN (SELECT id FROM VIP_customers);
```
* **Terms Used:**
  * `Single-row Subquery`: Subquery returning exactly one row/value.
  * `Multi-row Subquery`: Subquery returning multiple rows evaluated using operators like `IN`, `ANY`, `ALL`.

---

### 16. What is a constraint, and why use constraints?
* **Summary:** Constraints are rules enforced on table columns to maintain data accuracy, reliability, and integrity.
* **Code:**
```sql
CREATE TABLE account (
    id INT NOT NULL,
    balance DECIMAL CHECK (balance >= 0)
);
```
* **Terms Used:**
  * `Data Integrity`: Accuracy, consistency, and reliability of data stored in a database.

---

### 17. What SQL constraints do you know?
* **Summary:** Common constraints include `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK`, and `DEFAULT`.
* **Code:**
```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE NOT NULL,
    status VARCHAR(20) DEFAULT 'active'
);
```
* **Terms Used:**
  * `DEFAULT`: Assigns a automatic fallback value if no value is provided during insert.
  * `CHECK`: Ensures values in a column satisfy a specific Boolean condition.

---

### 18. What types of joins do you know?
* **Summary:** Standard joins include `INNER JOIN`, `LEFT (OUTER) JOIN`, `RIGHT (OUTER) JOIN`, `FULL (OUTER) JOIN`, `CROSS JOIN`, and `SELF JOIN`.
* **Code:**
```sql
-- LEFT JOIN returns all rows from A, and matching from B
SELECT * FROM TableA A LEFT JOIN TableB B ON A.id = B.id;
```
* **Terms Used:**
  * `INNER JOIN`: Returns only rows with matching values in both tables.
  * `OUTER JOIN`: Returns all rows from one or both tables, filling non-matching sides with NULLs.

---

### 19. What is a primary key in SQL?
* **Summary:** A column (or set of columns) that uniquely identifies each row in a table. It cannot contain `NULL` values and must be unique.
* **Code:**
```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100)
);
```
* **Terms Used:**
  * `Primary Key (PK)`: A unique identifier for every record in a table.

---

### 20. What is a unique key in SQL?
* **Summary:** Ensures all values in a column are distinct, but unlike a primary key, it allows a single `NULL` value (in most SQL engines).
* **Code:**
```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    ssn VARCHAR(11) UNIQUE
);
```
* **Terms Used:**
  * `Unique Constraint`: Prevents duplicate entries in a specific field.

---

### 21. What is a foreign key in SQL?
* **Summary:** A column in one table that points to the `PRIMARY KEY` of another table to maintain referential integrity between them.
* **Code:**
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT FOREIGN KEY REFERENCES customers(id)
);
```
* **Terms Used:**
  * `Referential Integrity`: Rule ensuring relationships between tables remain valid and consistent.

---

### 22. What is an SQL index?
* **Summary:** A data structure (usually a B-tree) created on table columns to drastically speed up data retrieval operations at the cost of slower writes.
* **Code:**
```sql
CREATE INDEX idx_user_email ON users(email);
```
* **Terms Used:**
  * `Index`: A lookup structure used by database query planners to find records quickly without full table scans.

---

### 23. What types of indexes do you know?
* **Summary:** Common index types include Clustered, Non-Clustered, Unique, Composite (multi-column), Full-Text, and B-Tree indexes.
* **Code:**
```sql
CREATE UNIQUE INDEX idx_emp_code ON employees(emp_code);
CREATE INDEX idx_name ON employees(last_name, first_name);
```
* **Terms Used:**
  * `Composite Index`: An index built across two or more columns simultaneously.

---

### 24. What is a schema?
* **Summary:** A schema is a logical collection of database objects including tables, views, indexes, stored procedures, and constraints.
* **Code:**
```sql
CREATE SCHEMA sales_department;
CREATE TABLE sales_department.orders (id INT);
```
* **Terms Used:**
  * `Schema`: A logical namespace that groups and organizes database objects.

---

### 25. What is an SQL operator?
* **Summary:** A reserved symbol or word used in SQL statements to perform operations like arithmetic, comparisons, or logical checks.
* **Code:**
```sql
SELECT * FROM products WHERE price >= 50 AND stock > 0;
```
* **Terms Used:**
  * `Operator`: Symbols like `=`, `>`, `<`, `AND`, `OR` used in evaluation expressions.

---

### 26. What types of SQL operators do you know?
* **Summary:** Arithmetic operators (`+`, `-`, `*`, `/`), Comparison operators (`=`, `!=`, `>`, `<`), Logical operators (`AND`, `OR`, `NOT`), and Special operators (`IN`, `BETWEEN`, `LIKE`, `IS NULL`).
* **Code:**
```sql
SELECT * FROM items WHERE price BETWEEN 10 AND 50 AND category IN ('Books', 'Toys');
```
* **Terms Used:**
  * `Logical Operator`: Evaluates two Boolean expressions to return true, false, or null.

---

### 27. What is a clause?
* **Summary:** A clause is a built-in component of an SQL statement used to filter, group, order, or manipulate data operations (e.g., `WHERE`, `GROUP BY`, `ORDER BY`).
* **Code:**
```sql
SELECT category, COUNT(*) 
FROM products 
WHERE status = 'Active' 
GROUP BY category;
```
* **Terms Used:**
  * `Clause`: Structural keywords in SQL that dictate how a statement processes data.

---

### 28. What are some common statements used with the SELECT query?
* **Summary:** Common companion clauses used with `SELECT` include `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, and `LIMIT` / `TOP`.
* **Code:**
```sql
SELECT dept_id, AVG(salary) 
FROM employees 
WHERE status = 'Active' 
GROUP BY dept_id 
HAVING AVG(salary) > 50000 
ORDER BY AVG(salary) DESC;
```
* **Terms Used:**
  * `HAVING`: Filters aggregate values after grouping occurs.
  * `GROUP BY`: Aggregates records sharing common column values.

---

### 29. How do you create a table in SQL?
* **Summary:** Using the `CREATE TABLE` command followed by table name, column definitions, data types, and constraint specifications.
* **Code:**
```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    hire_date DATE
);
```
* **Terms Used:**
  * `Data Type`: Defines what kind of value a column can hold (e.g., `INT`, `VARCHAR`, `DATE`).

---

### 30. How to update a table?
* **Summary:** Using the `UPDATE` statement combined with `SET` to modify column values, usually filtered with a `WHERE` clause.
* **Code:**
```sql
UPDATE employees 
SET salary = salary * 1.10 
WHERE department = 'Sales';
```
* **Terms Used:**
  * `SET`: Assigns new values to target columns during an update.

---

### 31. How to delete a table from a database?
* **Summary:** Using the `DROP TABLE` command, which permanently removes the table structure and all data inside it.
* **Code:**
```sql
DROP TABLE temp_logs;
```
* **Terms Used:**
  * `DROP`: A DDL command that permanently destroys database structures.

---

### 32. How to get the count of records in a table?
* **Summary:** Using the `COUNT()` aggregate function inside a `SELECT` statement.
* **Code:**
```sql
SELECT COUNT(*) FROM orders;
```
* **Terms Used:**
  * `COUNT(*)`: Aggregate function that returns the total count of rows matching criteria.

---

### 33. How to sort records in a table?
* **Summary:** Using the `ORDER BY` clause, followed by column names and sorting direction (`ASC` for ascending, `DESC` for descending).
* **Code:**
```sql
SELECT * FROM users ORDER BY created_at DESC;
```
* **Terms Used:**
  * `ASC / DESC`: Keywords specifying Ascending (default) or Descending sort order.

---

### 34. How to select all columns from a table?
* **Summary:** Using the asterisk wildcard operator (`*`) inside a `SELECT` statement.
* **Code:**
```sql
SELECT * FROM customers;
```
* **Terms Used:**
  * Wildcard (`*`): Symbol representing all columns in the source table.

---

### 35. How to select common records from two tables?
* **Summary:** By performing an `INNER JOIN` or by using the `INTERSECT` set operator.
* **Code:**
```sql
-- Method 1: INNER JOIN
SELECT A.id FROM TableA A INNER JOIN TableB B ON A.id = B.id;

-- Method 2: INTERSECT
SELECT id FROM TableA INTERSECT SELECT id FROM TableB;
```
* **Terms Used:**
  * `INTERSECT`: Set operator returning distinct rows common to both result sets.

---

### 36. What is the DISTINCT statement, and how do you use it?
* **Summary:** Used with `SELECT` to eliminate duplicate rows and return only unique values.
* **Code:**
```sql
SELECT DISTINCT country FROM customers;
```
* **Terms Used:**
  * `DISTINCT`: Keyword that filters out duplicate result rows.

---

### 37. What are relationships? Give some examples.
* **Summary:** Logical links established between database tables via primary and foreign keys. Examples: One-to-One (1:1), One-to-Many (1:N), and Many-to-Many (M:N).
* **Code:**
```sql
-- One-to-Many: 1 Customer has Many Orders
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT REFERENCES customers(id)
);
```
* **Terms Used:**
  * `Junction Table`: A bridge table used to implement Many-to-Many relationships.

---

### 38. What is a NULL value? How is it different from zero or a blank space?
* **Summary:** `NULL` represents a total absence of a value or unknown state. It is not equivalent to zero (0) or empty text (`''`).
* **Code:**
```sql
SELECT * FROM users WHERE phone_number IS NULL;
```
* **Terms Used:**
  * `NULL`: A special marker indicating missing or unknown data in SQL.
  * `Three-Valued Logic`: Logic system where expressions evaluate to TRUE, FALSE, or UNKNOWN (NULL).

---

### 39. What is the difference between SQL and NoSQL?
* **Summary:** SQL databases are relational, table-based, structured, and use fixed schemas. NoSQL databases are non-relational, document/key-value based, unstructured, and schema-flexible.
* **Code:**
```sql
-- SQL: Strict relational schema
SELECT * FROM users WHERE age > 21;
```
* **Terms Used:**
  * `Vertical Scaling`: Upgrading single-server hardware capacity (common in relational databases).
  * `Horizontal Scaling`: Adding more server machines into a cluster (common in NoSQL).

---

### 40. What are some common challenges when working with SQL databases?
* **Summary:** Challenges include slow query performance on massive datasets, handling database locks/deadlocks, schema migration risk, and scaling hardware vertically.
* **Code:**
```sql
-- Query tuning requires indexing columns used in joins and filters
EXPLAIN ANALYZE SELECT * FROM large_table WHERE indexed_col = 'val';
```
* **Terms Used:**
  * `Query Optimization`: Tuning SQL queries and indexes to execute with minimal resource usage.

---

## Section 4: Intermediate SQL Questions

### 41. What is a Common Table Expression (CTE)?
* **Summary:** A temporary, named result set defined using the `WITH` keyword that simplifies complex multi-step queries and can be referenced within the main query.
* **Code:**
```sql
WITH HighEarners AS (
    SELECT id, name, salary FROM employees WHERE salary > 100000
)
SELECT * FROM HighEarners WHERE name LIKE 'A%';
```
* **Terms Used:**
  * `CTE`: Common Table Expression, initialized using `WITH`.

---

### 42. What are window functions, and how do they differ from aggregate functions?
* **Summary:** Window functions perform calculations across a set of related table rows without collapsing rows together like `GROUP BY` aggregate functions do.
* **Code:**
```sql
SELECT name, dept_id, salary,
       AVG(salary) OVER (PARTITION BY dept_id) AS dept_avg
FROM employees;
```
* **Terms Used:**
  * `OVER()`: Clause defining the window frame for window functions.
  * `PARTITION BY`: Divides rows into query groups for window calculations.

---

### 43. What is the difference between RANK(), DENSE_RANK(), and ROW_NUMBER()?
* **Summary:** All assignment functions rank rows over an ordered partition: `ROW_NUMBER()` assigns strict sequential numbers; `RANK()` leaves gaps after tied values; `DENSE_RANK()` leaves no gaps after tied values.
* **Code:**
```sql
SELECT score,
       ROW_NUMBER() OVER (ORDER BY score DESC) AS num,
       RANK()       OVER (ORDER BY score DESC) AS rnk,
       DENSE_RANK() OVER (ORDER BY score DESC) AS drnk
FROM scores;
```
* **Terms Used:**
  * `Tie Handling`: How ranking functions evaluate identical values in an ordering clause.

---

### 44. What is a function in SQL?
* **Summary:** A routine that accepts input parameters, performs calculations or transformations, and returns a result value or table.
* **Code:**
```sql
SELECT UPPER(first_name), ROUND(salary, 2) FROM employees;
```
* **Terms Used:**
  * `Built-in Function`: Pre-defined SQL utility function like `UPPER()`, `COUNT()`, `ROUND()`.

---

### 45. What types of SQL functions do you know?
* **Summary:** SQL functions split broadly into Aggregate Functions (multi-row inputs producing single output) and Scalar Functions (single-row input producing single-row output).
* **Code:**
```sql
-- Scalar: LENGTH(), Aggregate: MAX()
SELECT MAX(LENGTH(product_name)) FROM products;
```
* **Terms Used:**
  * `Scalar`: Operates on individual values per row.
  * `Aggregate`: Collapses collections of rows into summary metrics.

---

### 46. What SQL aggregate functions do you know?
* **Summary:** Key aggregate functions include `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()`.
* **Code:**
```sql
SELECT department, COUNT(*), AVG(salary), MAX(salary)
FROM employees
GROUP BY department;
```
* **Terms Used:**
  * `Aggregation`: Combining multiple values into a single summary output value.

---

### 47. What SQL scalar functions do you know?
* **Summary:** Functions operating on individual items. Includes String (`UPPER`, `LOWER`, `CONCAT`, `SUBSTRING`), Numeric (`ROUND`, `ABS`), Date (`NOW`, `DATEDIFF`), and System functions.
* **Code:**
```sql
SELECT CONCAT(first_name, ' ', last_name), ROUND(price, 0) FROM products;
```
* **Terms Used:**
  * `Scalar Transformation`: Converting a row's column value into a newly formatted row output.

---

### 48. What are case manipulation functions in SQL?
* **Summary:** String functions used to alter character casing in text data: `LOWER()`, `UPPER()`, and `INITCAP()` (capitalizes first letter of each word).
* **Code:**
```sql
SELECT LOWER(email), UPPER(country) FROM users;
```
* **Terms Used:**
  * `Case Manipulation`: Normalizing character capitalization formats.

---

### 49. What are character manipulation functions in SQL?
* **Summary:** Functions used to inspect or edit text fields, such as `CONCAT()`, `SUBSTR()` / `SUBSTRING()`, `LENGTH()`, `TRIM()`, `REPLACE()`, and `INSTR()`.
* **Code:**
```sql
SELECT SUBSTRING(phone, 1, 3) AS area_code, LENGTH(notes) FROM contacts;
```
* **Terms Used:**
  * `String Parsing`: Extracting or replacing specific segments within textual data.

---

### 50. What is the difference between local and global variables?
* **Summary:** Local variables exist only within the specific block, stored procedure, or batch where defined. Global variables exist across session scope or system-wide.
* **Code:**
```sql
-- SQL Server Local Variable Syntax
DECLARE @LocalVar INT = 10;
SELECT @LocalVar;
```
* **Terms Used:**
  * `Scope`: The accessible lifetime boundary of a declared variable.

---

### 51. What is the difference between SQL and PL/SQL?
* **Summary:** SQL is a declarative database query language. PL/SQL (Procedural Language/SQL) is Oracle’s proprietary extension adding procedural elements like loops, branches, and variables.
* **Code:**
```sql
-- PL/SQL block structure
BEGIN
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
    COMMIT;
END;
```
* **Terms Used:**
  * `Procedural Language`: Programming model using step-by-step logic, condition blocks, and loops.

---

### 52. What is the difference between LEFT JOIN and LEFT OUTER JOIN?
* **Summary:** There is zero functional difference. `LEFT JOIN` is shorthand syntax for `LEFT OUTER JOIN`.
* **Code:**
```sql
-- Both queries yield identical engine execution plans
SELECT * FROM A LEFT JOIN B ON A.id = B.id;
SELECT * FROM A LEFT OUTER JOIN B ON A.id = B.id;
```
* **Terms Used:**
  * `Syntactic Sugar`: Alternative code syntax that makes language easier to write without altering behavior.

---

### 53. What is indexing in SQL, and how does it improve performance?
* **Summary:** Indexing creates a sorted lookup data structure (e.g., B-Tree) mapping index keys to physical row addresses, avoiding slow full-table scans during search operations.
* **Code:**
```sql
CREATE INDEX idx_user_id ON orders(customer_id);
```
* **Terms Used:**
  * `Table Scan`: An expensive database operation checking every single row sequentially.
  * `Index Seek`: A fast B-Tree traversal directly identifying exact matching rows.

---

### 54. What is a stored procedure, and how is it different from a function?
* **Summary:** A stored procedure is a compiled group of reusable SQL statements that performs complex workflows and may alter database state. Unlike functions, procedures don't have to return a value and cannot be called directly within a `SELECT` statement.
* **Code:**
```sql
CREATE PROCEDURE UpdateSalary(emp_id INT, new_sal DECIMAL)
AS
BEGIN
    UPDATE employees SET salary = new_sal WHERE id = emp_id;
END;
```
* **Terms Used:**
  * `Stored Procedure`: Prepared SQL logic stored on the database server for execution.

---

### 55. What is the default data ordering with the ORDER BY statement, and how do you change it?
* **Summary:** The default sort order is Ascending (`ASC`). You change it to Descending by explicitly adding `DESC`.
* **Code:**
```sql
SELECT * FROM products ORDER BY price DESC;
```
* **Terms Used:**
  * `Ascending (ASC)`: Sorts values lowest-to-highest or A-to-Z.
  * `Descending (DESC)`: Sorts values highest-to-lowest or Z-to-A.

---

### 56. What are SQL set operators?
* **Summary:** Set operators combine results from two distinct `SELECT` queries into a single result set. Includes `UNION`, `UNION ALL`, `INTERSECT`, and `EXCEPT` / `MINUS`.
* **Code:**
```sql
SELECT name FROM employees
UNION ALL
SELECT name FROM contractors;
```
* **Terms Used:**
  * `Set Theory`: Mathematical foundations governing operation sets (unions, intersections, differences).

---

### 57. What operator is used in the query for pattern matching?
* **Summary:** The `LIKE` operator, using wildcard characters `%` (matches multiple characters) and `_` (matches exactly one character).
* **Code:**
```sql
SELECT * FROM users WHERE email LIKE 'j%@gmail.com';
```
* **Terms Used:**
  * `Wildcard`: Symbols (`%`, `_`) representing unknown characters in string search patterns.

---

### 58. What is the difference between a primary key and a unique key in SQL?
* **Summary:** A primary key uniquely identifies rows, prohibits `NULL` values completely, and a table allows only one primary key. A unique key prevents duplicates, usually allows one `NULL`, and a table can have multiple unique keys.
* **Code:**
```sql
CREATE TABLE account (
    acc_id INT PRIMARY KEY,         -- Only 1 allowed, no NULLs
    email VARCHAR(100) UNIQUE,      -- Multiple allowed, permits NULL
    ssn VARCHAR(11) UNIQUE
);
```
* **Terms Used:**
  * `Entity Integrity`: Rule ensuring each table row remains distinctly identifiable.

---

### 59. What is a SQL composite primary key?
* **Summary:** A primary key consisting of two or more combined columns to uniquely identify each row in a table.
* **Code:**
```sql
CREATE TABLE order_items (
    order_id INT,
    item_id INT,
    quantity INT,
    PRIMARY KEY (order_id, item_id)
);
```
* **Terms Used:**
  * `Composite Key`: Multi-column primary identifier.

---

### 60. What is the typical order of SQL clauses in a SELECT statement?
* **Summary:** Written order: `SELECT` -> `FROM` -> `JOIN` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `ORDER BY` -> `LIMIT`.
* **Code:**
```sql
SELECT dept, COUNT(*)
FROM employees
WHERE age > 25
GROUP BY dept
HAVING COUNT(*) > 5
ORDER BY dept ASC
LIMIT 10;
```
* **Terms Used:**
  * `Statement Syntax`: The mandatory sequence required when writing SQL queries.

---

### 61. In which order does the interpreter execute the common statements in the SELECT query?
* **Summary:** Execution order: 1. `FROM` / `JOIN` -> 2. `WHERE` -> 3. `GROUP BY` -> 4. `HAVING` -> 5. `SELECT` -> 6. `DISTINCT` -> 7. `ORDER BY` -> 8. `LIMIT`.
* **Code:**
```sql
-- Execution order starts at FROM/WHERE long before SELECT projections happen
SELECT dept, AVG(salary) AS avg_sal
FROM employees
WHERE status = 'Active'
GROUP BY dept
HAVING AVG(salary) > 40000;
```
* **Terms Used:**
  * `Logical Query Processing`: Internal sequence database engines follow to compute results.

---

### 62. What is a view in SQL?
* **Summary:** A virtual table based on the stored result of a pre-written `SELECT` statement. It doesn't store physical data itself.
* **Code:**
```sql
CREATE VIEW active_users AS
SELECT id, name, email FROM users WHERE status = 'Active';

SELECT * FROM active_users;
```
* **Terms Used:**
  * `Virtual Table`: An abstraction presenting query results as a queryable table structure.

---

### 63. Can we create a view based on another view in SQL?
* **Summary:** Yes, this is known as nested views. The new view queries the existing view as its data source.
* **Code:**
```sql
CREATE VIEW US_Active_Users AS
SELECT * FROM active_users WHERE country = 'USA';
```
* **Terms Used:**
  * `Nested View`: Constructing a secondary view object directly dependent on an antecedent view.

---

### 64. Can we still use a view if the original table is deleted?
* **Summary:** No. When the base table is dropped, the view becomes invalid/broken and queries against it will throw errors.
* **Code:**
```sql
DROP TABLE users;
-- Querying active_users view now fails with an error!
```
* **Terms Used:**
  * `Dependency`: Objects relying on underlying database artifacts to function correctly.

---

### 65. What types of SQL relationships do you know?
* **Summary:** One-to-One (1:1), One-to-Many (1:N or Many-to-One), and Many-to-Many (M:N).
* **Code:**
```sql
-- Many-to-Many requires a junction table:
CREATE TABLE student_courses (
    student_id INT REFERENCES students(id),
    course_id INT REFERENCES courses(id),
    PRIMARY KEY (student_id, course_id)
);
```
* **Terms Used:**
  * `Cardinality`: Defines how many instances of one entity relate to instances of another entity.

---

### 66. What are the possible values of a BOOLEAN data field?
* **Summary:** Three possibilities under SQL's 3-valued logic: `TRUE`, `FALSE`, and `NULL` (unknown).
* **Code:**
```sql
CREATE TABLE tasks (
    task_id INT,
    is_completed BOOLEAN DEFAULT FALSE
);
```
* **Terms Used:**
  * `Boolean`: Logic data type taking truth values.

---

### 67. What is normalization in SQL?
* **Summary:** The process of organizing data tables to minimize redundant data and eliminate data anomalies during inserts, updates, and deletes (1NF, 2NF, 3NF, BCNF).
* **Code:**
```sql
-- Normalized: Splitting user info and user orders into two linked tables
CREATE TABLE users (id INT PRIMARY KEY, name VARCHAR(50));
CREATE TABLE orders (id INT PRIMARY KEY, user_id INT REFERENCES users(id));
```
* **Terms Used:**
  * `Data Redundancy`: Unnecessary duplication of data across database fields.
  * `Normal Forms (1NF, 2NF, 3NF)`: Progressive rules evaluating table design quality.

---

### 68. What is denormalization in SQL?
* **Summary:** The deliberate practice of adding redundant data back into normalized tables to reduce costly `JOIN` operations and optimize query read performance.
* **Code:**
```sql
-- Denormalized: Storing customer_name directly inside the orders table to avoid JOINs
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    customer_name VARCHAR(100) -- Redundant column added for faster reads
);
```
* **Terms Used:**
  * `Read Optimization`: Designing schemas specifically to favor fast retrieval speed over update efficiency.

---

### 69. What is the difference between renaming a column and giving an alias to it?
* **Summary:** Renaming physically alters column names permanently in table metadata (`ALTER TABLE`). An alias assigns a temporary name for query output formatting without modifying table schema.
* **Code:**
```sql
-- Permanent schema change:
ALTER TABLE users RENAME COLUMN fname TO first_name;

-- Temporary query alias:
SELECT fname AS first_name FROM users;
```
* **Terms Used:**
  * `DDL vs DQL`: Structural schema modification vs temporary query execution output.

---

### 70. What is the difference between nested and correlated subqueries?
* **Summary:** A nested subquery runs independently of the outer query once. A correlated subquery references columns from the outer query, running repeatedly once for every row processed by the outer query.
* **Code:**
```sql
-- Correlated Subquery: Uses e1.dept_id from outer query
SELECT * FROM employees e1
WHERE salary > (
    SELECT AVG(salary) FROM employees e2 WHERE e2.dept_id = e1.dept_id
);
```
* **Terms Used:**
  * `Correlated Subquery`: Subquery evaluated repeatedly using outer query values.

---

### 71. What is the difference between clustered and non-clustered indexes?
* **Summary:** A clustered index determines the physical storage order of table data (only 1 per table). A non-clustered index creates a separate lookup structure with pointers to actual rows (multiple allowed per table).
* **Code:**
```sql
-- Primary Key automatically forms the Clustered Index on most engines
CREATE TABLE books (book_id INT PRIMARY KEY);

-- Non-clustered index on secondary column
CREATE INDEX idx_title ON books(title);
```
* **Terms Used:**
  * `Clustered Index`: Index determining physical table row ordering on disk storage.
  * `Non-Clustered Index`: Index maintaining dedicated key list pointing back to row locations.

---

### 72. What is the CASE() function?
* **Summary:** SQL’s conditional expression logic equivalent to IF-THEN-ELSE statements inside queries.
* **Code:**
```sql
SELECT product_name,
       CASE 
           WHEN price > 100 THEN 'Expensive'
           WHEN price > 50 THEN 'Moderate'
           ELSE 'Cheap'
       END AS price_category
FROM products;
```
* **Terms Used:**
  * `Conditional Logic`: Executing expressions based on meeting defined boolean criteria.

---

### 73. What is the difference between the DELETE and TRUNCATE statements?
* **Summary:** `DELETE` is a DML command that removes specific or all rows one by one, logs row deletes, and supports `WHERE` clauses. `TRUNCATE` is a DDL command that deallocates entire table storage pages quickly, cannot use `WHERE`, and is much faster.
* **Code:**
```sql
-- DELETE: Logged, filterable
DELETE FROM logs WHERE log_date < '2023-01-01';

-- TRUNCATE: Unfiltered, fast, resets identities
TRUNCATE TABLE logs;
```
* **Terms Used:**
  * `DML vs DDL`: Data Manipulation vs Data Definition level commands.
  * `Transaction Log`: Database activity log maintaining records for rollbacks.

---

### 74. What is the difference between the DROP and TRUNCATE statements?
* **Summary:** `TRUNCATE` deletes all data rows from a table while preserving the table schema structure. `DROP` deletes both data rows AND the table schema permanently from the database.
* **Code:**
```sql
-- Table structure survives, data vanishes:
TRUNCATE TABLE temp_data;

-- Both structure and data vanish completely:
DROP TABLE temp_data;
```
* **Terms Used:**
  * `Table Definition`: The blueprint schema specifying table columns and data types.

---

### 75. What is the difference between the HAVING and WHERE statements?
* **Summary:** `WHERE` filters individual input rows before aggregation occurs. `HAVING` filters grouped aggregate results after `GROUP BY` execution.
* **Code:**
```sql
SELECT dept_id, COUNT(*)
FROM employees
WHERE status = 'Active'         -- Row filter
GROUP BY dept_id
HAVING COUNT(*) > 10;            -- Aggregate filter
```
* **Terms Used:**
  * `Pre-aggregation vs Post-aggregation`: Filtering before vs after row group calculations occur.

---

### 76. How do you add a record to a table?
* **Summary:** Using the `INSERT INTO` statement specifying target table, columns, and values.
* **Code:**
```sql
INSERT INTO users (id, name, email) 
VALUES (1, 'Alice', 'alice@example.com');
```
* **Terms Used:**
  * `INSERT INTO`: Command specifying table record additions.

---

### 77. How do you delete a record from a table?
* **Summary:** Using the `DELETE FROM` statement paired with a matching `WHERE` clause condition.
* **Code:**
```sql
DELETE FROM users WHERE id = 1;
```
* **Terms Used:**
  * `DELETE`: Command removing specified data rows.

---

### 78. How do you add a column to a table?
* **Summary:** Using the `ALTER TABLE` statement combined with the `ADD` clause.
* **Code:**
```sql
ALTER TABLE users ADD phone_number VARCHAR(15);
```
* **Terms Used:**
  * `ALTER TABLE`: Command modifying existing table structural definitions.

---

### 79. How do you rename a column of a table?
* **Summary:** Using `ALTER TABLE` with `RENAME COLUMN ... TO ...`.
* **Code:**
```sql
ALTER TABLE users RENAME COLUMN phone_number TO contact_number;
```
* **Terms Used:**
  * `Schema Modification`: Updating database structural metadata definitions.

---

### 80. How do you delete a column from a table?
* **Summary:** Using `ALTER TABLE` combined with `DROP COLUMN`.
* **Code:**
```sql
ALTER TABLE users DROP COLUMN contact_number;
```
* **Terms Used:**
  * `DROP COLUMN`: Sub-clause stripping structural fields from tables.

---

### 81. How do you select all even or all odd records in a table?
* **Summary:** Using the modulo operator (`%` or `MOD()`) on an ID or row number field.
* **Code:**
```sql
-- Select Even IDs
SELECT * FROM users WHERE id % 2 = 0;

-- Select Odd IDs
SELECT * FROM users WHERE id % 2 != 0;
```
* **Terms Used:**
  * Modulo Operator (`%`): Arithmetic operation returning the remainder after division.

---

### 82. How to prevent duplicate records when making a query?
* **Summary:** Using the `DISTINCT` keyword or grouping results using `GROUP BY`.
* **Code:**
```sql
SELECT DISTINCT city FROM customers;
```
* **Terms Used:**
  * `Deduplication`: Stripping identical output rows from result sets.

---

### 83. How do you insert many rows in a table?
* **Summary:** Combining multiple comma-separated value tuples into a single `INSERT INTO` statement.
* **Code:**
```sql
INSERT INTO products (id, name) VALUES 
(1, 'Laptop'),
(2, 'Phone'),
(3, 'Tablet');
```
* **Terms Used:**
  * `Bulk Insert`: Passing multiple row tuples inside one statement for performance optimization.

---

### 84. How do you find the nth highest value in a column of a table?
* **Summary:** Using subqueries with `LIMIT` / `OFFSET` or using the `DENSE_RANK()` window function.
* **Code:**
```sql
-- Finding 3rd highest salary using DENSE_RANK()
WITH RankedSalaries AS (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) as rnk
    FROM employees
)
SELECT DISTINCT salary FROM RankedSalaries WHERE rnk = 3;
```
* **Terms Used:**
  * `OFFSET`: Skipping a designated count of initial query rows.
  * `DENSE_RANK()`: Ranking function mapping positions without value gaps.

---

### 85. How do you find the values in a text column of a table that start with a certain letter?
* **Summary:** Using the `LIKE` pattern matching operator combined with the `%` wildcard character.
* **Code:**
```sql
SELECT * FROM customers WHERE name LIKE 'A%';
```
* **Terms Used:**
  * `Pattern Matching`: String matching using wildcard rules.

---

### 86. How do you find the last id in a table?
* **Summary:** Using `MAX(id)` or sorting `ORDER BY id DESC` with a limit of 1.
* **Code:**
```sql
SELECT MAX(id) FROM orders;
```
* **Terms Used:**
  * `MAX()`: Aggregate function returning the largest scalar value in a group.

---

### 87. How to select random rows from a table?
* **Summary:** Ordering rows by random generator functions like `RAND()`, `RANDOM()`, or `NEWID()` and taking a limit.
* **Code:**
```sql
-- PostgreSQL / SQLite
SELECT * FROM products ORDER BY RANDOM() LIMIT 5;

-- MySQL
SELECT * FROM products ORDER BY RAND() LIMIT 5;
```
* **Terms Used:**
  * Pseudo-Random Function: Engine specific function yielding dynamic numeric outputs for sorting.

---

## Section 5: Scenario-Based SQL Questions

### 88. How do you find and remove duplicate records from a table?
* **Summary:** Find duplicates using `GROUP BY` and `HAVING COUNT(*) > 1`. Remove duplicates by isolating distinct IDs using CTEs paired with window functions like `ROW_NUMBER()`.
* **Code:**
```sql
WITH CTE AS (
    SELECT id, email,
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS row_num
    FROM users
)
DELETE FROM users WHERE id IN (SELECT id FROM CTE WHERE row_num > 1);
```
* **Terms Used:**
  * `ROW_NUMBER()`: Assigns ordinal increments per partition.
  * `PARTITION BY`: Divides input sets into window calculation groupings.

---

### 89. How do you calculate a running total (cumulative sum)?
* **Summary:** Using the `SUM()` aggregate function as a window function paired with `OVER (ORDER BY ...)`.
* **Code:**
```sql
SELECT sales_date, amount,
       SUM(amount) OVER (ORDER BY sales_date) AS running_total
FROM sales;
```
* **Terms Used:**
  * `Cumulative Sum`: Progressive summation accumulating previous row values sequentially.

---

### 90. How do you find employees who earn more than the average salary in their department?
* **Summary:** Use a correlated subquery comparing individual employee salary against department average, or a CTE calculating department averages.
* **Code:**
```sql
SELECT e.name, e.dept_id, e.salary
FROM employees e
WHERE e.salary > (
    SELECT AVG(salary) 
    FROM employees 
    WHERE dept_id = e.dept_id
);
```
* **Terms Used:**
  * `Correlated Query`: Subquery re-calculated row-by-row against parent query criteria.

---

### 91. How do you find gaps in a sequence of numbers (e.g., missing invoice numbers)?
* **Summary:** Use the `LEAD()` window function to evaluate the next sequential value and check if `next_value - current_value > 1`.
* **Code:**
```sql
WITH SequenceGaps AS (
    SELECT id AS current_id,
           LEAD(id) OVER (ORDER BY id) AS next_id
    FROM invoices
)
SELECT current_id + 1 AS gap_start, next_id - 1 AS gap_end
FROM SequenceGaps
WHERE next_id - current_id > 1;
```
* **Terms Used:**
  * `LEAD()`: Analytic window function fetching column values from subsequent rows.

---

### 92. How do you find customers who made purchases in consecutive months?
* **Summary:** Extract purchase months using date truncation/formatting, use `LAG()` to pull previous purchase month, and check for 1-month differences.
* **Code:**
```sql
WITH MonthlyPurchases AS (
    SELECT DISTINCT customer_id, 
           DATE_TRUNC('month', purchase_date) AS purchase_month
    FROM purchases
),
ConsecutiveCheck AS (
    SELECT customer_id, purchase_month,
           LAG(purchase_month) OVER (PARTITION BY customer_id ORDER BY purchase_month) AS prev_month
    FROM MonthlyPurchases
)
SELECT DISTINCT customer_id 
FROM ConsecutiveCheck 
WHERE purchase_month = prev_month + INTERVAL '1 month';
```
* **Terms Used:**
  * `LAG()`: Window function fetching values from preceding rows.
  * `DATE_TRUNC()`: Function truncating timestamp metrics to designated precision levels.

---

### 93. How do you pivot data from rows to columns?
* **Summary:** Transform row values into column headers using conditional aggregation (`SUM(CASE ...)` or `FILTER`) or native `PIVOT` statements.
* **Code:**
```sql
SELECT year,
       SUM(CASE WHEN quarter = 'Q1' THEN sales ELSE 0 END) AS Q1_Sales,
       SUM(CASE WHEN quarter = 'Q2' THEN sales ELSE 0 END) AS Q2_Sales
FROM quarterly_sales
GROUP BY year;
```
* **Terms Used:**
  * `Pivot`: Converting vertical tall data structures into horizontal wide report formats.

---

### 94. How do you find the top 3 products by sales in each category?
* **Summary:** Rank products within each category using `DENSE_RANK()` or `ROW_NUMBER()` in a CTE, then filter for `rank <= 3`.
* **Code:**
```sql
WITH RankedProducts AS (
    SELECT category_id, product_name, sales,
           DENSE_RANK() OVER (PARTITION BY category_id ORDER BY sales DESC) AS rnk
    FROM products
)
SELECT category_id, product_name, sales 
FROM RankedProducts 
WHERE rnk <= 3;
```
* **Terms Used:**
  * `PARTITION BY`: Divides data window calculations into localized groups.

---

### 95. What are ACID properties in database transactions?
* **Summary:** ACID ensures database transactional reliability:
  * **Atomicity:** All-or-nothing completion.
  * **Consistency:** Keeps database state valid according to constraints.
  * **Isolation:** Concurrent transactions don't interfere with each other.
  * **Durability:** Committed changes persist even after system crashes.
* **Code:**
```sql
BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
    UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- All succeeds or ROLLBACK triggers
```
* **Terms Used:**
  * `ACID`: Atomicity, Consistency, Isolation, Durability guarantee principles.
  * `Transaction`: A logical unit of database work consisting of one or more steps.

---

### 96. What is a deadlock, and how do you prevent it?
* **Summary:** A deadlock occurs when two or more concurrent transactions block each other indefinitely by holding locks on resources the other needs. Prevent by acquiring locks in a consistent order and keeping transactions short.
* **Code:**
```sql
-- Prevention Strategy: Ensure consistent lock ordering across transactions
-- Transaction A & Transaction B both update Account 1 before Account 2
BEGIN;
UPDATE accounts SET balance = balance - 50 WHERE id = 1;
UPDATE accounts SET balance = balance + 50 WHERE id = 2;
COMMIT;
```
* **Terms Used:**
  * `Deadlock`: Cyclic dependency lock lockup where operations wait indefinitely on each other.
  * `Locking`: Concurrency control mechanisms preventing conflicting data updates.

---

### 97. How do you optimize a slow-running SQL query?
* **Summary:** Add indexes on search/join columns, avoid `SELECT *`, eliminate unindexed wildcards (like `%term`), examine execution plans (`EXPLAIN`), and replace correlated subqueries with JOINs/CTEs.
* **Code:**
```sql
-- Analyze query execution plan
EXPLAIN ANALYZE 
SELECT name, price FROM products WHERE category_id = 5;
```
* **Terms Used:**
  * `Execution Plan`: The query engine's computational blueprint strategy for retrieving target data.

---

### 98. How do you handle NULL values in calculations and comparisons?
* **Summary:** Handle `NULL` explicitly using functions like `COALESCE()`, `IFNULL()`, or `NVL()`, and evaluate condition states with `IS NULL` / `IS NOT NULL`.
* **Code:**
```sql
SELECT name, COALESCE(commission, 0) + salary AS total_comp 
FROM employees;
```
* **Terms Used:**
  * `COALESCE()`: Function returning the first non-null input value in its parameter list.

---

### 99. How do you find the longest streak of consecutive login days for a user?
* **Summary:** Form group identifiers by subtracting dense rank values from event dates (`login_date - ROW_NUMBER()`), group by that resulting identifier, count days per block, and find the maximum block length.
* **Code:**
```sql
WITH DistinctLogins AS (
    SELECT DISTINCT user_id, CAST(login_time AS DATE) AS login_date
    FROM user_logins
),
GroupedStreaks AS (
    SELECT user_id, login_date,
           login_date - (ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) * INTERVAL '1 day') AS streak_group
    FROM DistinctLogins
),
StreakLengths AS (
    SELECT user_id, COUNT(*) AS streak_length
    FROM GroupedStreaks
    GROUP BY user_id, streak_group
)
SELECT user_id, MAX(streak_length) AS longest_streak
FROM StreakLengths
GROUP BY user_id;
```
* **Terms Used:**
  * `Gaps and Islands`: Classic SQL problem pattern grouping consecutive contiguous sequence runs ("islands") across dataset breaks ("gaps").

---
