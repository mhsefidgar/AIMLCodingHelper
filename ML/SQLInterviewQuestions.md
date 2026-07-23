# SQL Questions and Answers by Mohamamd Sefidgar

## Section 1: General SQL Questions

### 1. What is SQL?
SQL (Structured Query Language) is the standard language used to communicate with, manage, and manipulate relational databases.

```sql
-- Retrieve all details for active employees in department 10
SELECT * 
FROM employees 
WHERE dept_id = 10 AND status = 'ACTIVE';
```

**Code Logic:**  
The query uses the `SELECT *` clause to request every column from the `employees` table. The `WHERE` clause filters the rows so that only records where `dept_id` equals `10` and `status` equals `'ACTIVE'` are returned from the database engine.

**Terms Used:**
  * `SQL`: Structured Query Language, the standardized programming language used to manage relational databases.
  * `SELECT`: The SQL keyword used to retrieve data from a database table.
  * `WHERE`: A clause used to filter database query records based on specific condition criteria.

---

### 2. What are SQL dialects? Give some examples.
Dialects are different versions or extensions of standard SQL developed by different database vendors (e.g., PostgreSQL, MySQL, SQL Server, Oracle, SQLite).

```sql
-- PostgreSQL specific string concatenation using double pipes
SELECT 'User: ' || 'Alice' AS user_label;

-- MySQL specific string concatenation using CONCAT function
SELECT CONCAT('User: ', 'Alice') AS user_label;
```

**Code Logic:**  
Both queries aim to merge two string literals into a single output field named `user_label`. The first query uses PostgreSQL's `||` operator, while the second query uses MySQL's built-in `CONCAT()` function to achieve the identical formatted result `'User: Alice'`.

**Terms Used:**
  * `SQL Dialect`: A database vendor's specific implementation and expansion of the ANSI SQL standard.
  * `CONCAT`: A string function used to join two or more text values into one continuous string.

---

### 3. What are the main applications of SQL?
SQL is used to create and modify database schemas, read data, update existing records, delete unwanted records, and manage access permissions.

```sql
-- Insert a new client with explicit ID and name
INSERT INTO clients (id, name, balance) VALUES (101, 'Acme Corp', 5000.00);

-- Update the account balance for the inserted client
UPDATE clients SET balance = 5500.00 WHERE id = 101;
```

**Code Logic:**  
The `INSERT INTO` statement adds a new row to the `clients` table containing an ID of `101`, name `'Acme Corp'`, and balance `5000.00`. Following that, the `UPDATE` statement modifies the `balance` column of that specific row to `5500.00` by locating it via `WHERE id = 101`.

**Terms Used:**
  * `INSERT INTO`: An SQL command used to add new rows of data into a database table.
  * `UPDATE`: An SQL command used to modify existing column values within a database table.

---

## Section 2: SQL Questions for Beginners

### 4. What is an SQL statement?
An SQL statement is a valid instruction/command written in SQL that the database engine can parse and execute.

```sql
-- Remove a temporary logs table safely if it exists in the schema
DROP TABLE IF EXISTS audit_logs_temp;
```

**Code Logic:**  
This DDL statement instructs the database server to check if a table named `audit_logs_temp` exists in the active schema. If present, the engine deletes the table structure and its physical data storage pages permanently.

**Terms Used:**
  * `SQL Statement`: A complete, executable command sent to a database engine to perform an action.
  * `DROP TABLE`: A Data Definition Language (DDL) command that permanently destroys an existing table.

---

### 5. What is an SQL query?
A query is a specific SQL statement written to retrieve, aggregate, or modify data within database tables.

```sql
-- Fetch student names and ages for active students older than 18
SELECT name, age 
FROM students 
WHERE age > 18 AND status = 'ENROLLED';
```

**Code Logic:**  
The engine inspects the `students` table, evaluates each row against the conditions in the `WHERE` clause (`age > 18` and `status = 'ENROLLED'`), and projects only the `name` and `age` columns for matching rows into the final result set.

**Terms Used:**
  * `SQL Query`: An SQL instruction specifically designed to request and retrieve data from a database.
  * `WHERE`: A filtering clause that limits returned query rows based on conditional expressions.

---

### 6. What is an SQL subquery?
A subquery (or inner query) is a query embedded inside another SQL query to evaluate intermediate values.

```sql
-- Retrieve employees earning more than the overall company average salary
SELECT name, salary 
FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees);
```

**Code Logic:**  
The inner subquery `(SELECT AVG(salary) FROM employees)` executes first to compute a single overall average salary (e.g., `$65,000`). The outer query then compares each employee's `salary` against this calculated benchmark value to filter the final record list.

**Terms Used:**
  * `Subquery`: A nested query embedded inside a primary SQL statement.
  * `AVG`: An aggregate function that calculates the arithmetic mean of a numeric column.

---

### 7. What is an SQL join?
A join is an operation used to combine rows from two or more tables based on a common related column between them.

```sql
-- Combine customer info with order details matching customer IDs
SELECT orders.order_id, customers.customer_name, orders.total_amount
FROM orders 
JOIN customers ON orders.customer_id = customers.id;
```

**Code Logic:**  
The query reads the `orders` table and performs an inner join with the `customers` table. The `ON` clause matches records where `orders.customer_id` equals `customers.id`, creating merged output rows containing data fields from both source tables.

**Terms Used:**
  * `JOIN`: A clause used to combine fields from two tables based on matching key columns.
  * `ON`: The keyword specifying the join condition linking primary and foreign key columns.

---

### 8. What is an SQL comment?
Comments are notes added into SQL code to explain logic. They are ignored by the query executor.

```sql
-- Fetch active inventory count for warehouse 5
SELECT item_id, quantity 
FROM inventory 
WHERE warehouse_id = 5; /* Filter for main warehouse */
```

**Code Logic:**  
The database parser skips the single-line comment beginning with `--` and the multi-line comment surrounded by `/* ... */`. It executes only the active `SELECT` statement to pull inventory items stored in warehouse `5`.

**Terms Used:**
  * `Comment`: Explanatory text inserted inside source code that is ignored during query execution.
  * `SELECT`: The command used to define which table columns to retrieve.

---

### 9. What is an SQL alias?
An alias is a temporary name assigned to a table or column to make the query cleaner and easier to read.

```sql
-- Rename column outputs and assign a short table alias
SELECT e.first_name AS employee_name, e.salary * 12 AS annual_salary 
FROM employees AS e;
```

**Code Logic:**  
The statement assigns `e` as a short table alias for `employees`. In the projection list, it renames `first_name` to `employee_name` and calculates a new computed column `salary * 12`, aliasing the calculated output header as `annual_salary`.

**Terms Used:**
  * `Alias`: A temporary substitute name given to a column or table within a query.
  * `AS`: The SQL keyword used to define an explicit column or table alias name.

---

## Section 3: Technical SQL Questions

### 10. What types of SQL commands do you know?
SQL commands fall into 5 main categories: DDL (structure), DML (data manipulation), DQL (querying), DCL (permissions), and TCL (transactions).

```sql
-- DDL: Define structure | DML: Insert row | DQL: Read row
CREATE TABLE roles (role_id INT PRIMARY KEY, title VARCHAR(30));
INSERT INTO roles VALUES (1, 'Administrator');
SELECT * FROM roles;
```

**Code Logic:**  
First, a DDL statement (`CREATE TABLE`) builds the schema structure for `roles`. Second, a DML statement (`INSERT INTO`) populates a row with `role_id = 1` and `title = 'Administrator'`. Third, a DQL statement (`SELECT`) reads all rows back.

**Terms Used:**
  * `DDL`: Data Definition Language commands (`CREATE`, `ALTER`, `DROP`) managing schema design.
  * `DML`: Data Manipulation Language commands (`INSERT`, `UPDATE`, `DELETE`) modifying stored rows.
  * `DQL`: Data Query Language commands (`SELECT`) retrieving stored information.

---

### 11. Give some examples of common SQL commands.
Basic commands include `CREATE`, `ALTER`, `DROP` (DDL); `INSERT`, `UPDATE`, `DELETE` (DML); `SELECT` (DQL); `GRANT`, `REVOKE` (DCL); `COMMIT`, `ROLLBACK` (TCL).

```sql
-- Create table structure, populate values, and query results
CREATE TABLE system_config (config_id INT, config_value VARCHAR(50));
INSERT INTO system_config VALUES (10, 'ENABLED');
SELECT config_value FROM system_config WHERE config_id = 10;
```

**Code Logic:**  
`CREATE TABLE` instantiates `system_config` with two typed columns. `INSERT INTO` assigns literal row values `10` and `'ENABLED'`. `SELECT` retrieves `'ENABLED'` by filtering `config_id = 10`.

**Terms Used:**
  * `CREATE TABLE`: A DDL command creating a new table in the current database schema.
  * `INSERT INTO`: A DML command adding data records to an established table.
  * `SELECT`: A DQL command fetching specific data fields from table storage.

---

### 12. What is DBMS, and what types of DBMS do you know?
A Data Base Management System is software used to store, retrieve, and manage data. Types include Relational (RDBMS), NoSQL (Document, Key-Value), Object-Oriented, and Hierarchical.

```sql
-- Relational DBMS query pulling structured user data
SELECT user_id, email 
FROM app_users 
WHERE account_status = 'ACTIVE';
```

**Code Logic:**  
The relational database management system processes the SQL request by scanning the structured table index for `app_users`, matching rows with `account_status = 'ACTIVE'`, and returning the tabular result containing `user_id` and `email`.

**Terms Used:**
  * `DBMS`: Database Management System; core application software handling database storage and access.
  * `SELECT`: Structured query clause used to extract matching records from relational tables.

---

### 13. What is RDBMS? Give some examples of RDBMS.
A Relational DBMS organizes data into structured tables linked by relationships (keys). Examples: PostgreSQL, MySQL, Oracle, SQL Server, SQLite.

```sql
-- Extract employee details linked to department names
SELECT e.emp_name, d.dept_name 
FROM employees e 
JOIN departments d ON e.dept_id = d.dept_id;
```

**Code Logic:**  
The RDBMS engine joins two distinct tables (`employees` and `departments`) using the common relational foreign key column `dept_id`, outputting paired values corresponding to employee names and their respective organizational departments.

**Terms Used:**
  * `RDBMS`: Relational Database Management System organized around relational tabular data structures.
  * `JOIN`: An operational keyword linking related database tables on designated key fields.

---

### 14. What are tables and fields in SQL?
A table is a structured collection of related data composed of rows (records) and columns (fields/attributes).

```sql
-- Create table defining specific fields and data types
CREATE TABLE user_accounts (
    account_id INT,          -- Field 1: Integer identifier
    username VARCHAR(50)     -- Field 2: String text field
);
```

**Code Logic:**  
The query executes a schema creation step for a table named `user_accounts`. It establishes two dedicated fields (columns): `account_id` configured to hold integer numbers and `username` configured for text strings up to 50 characters long.

**Terms Used:**
  * `Table`: A structured 2D database object consisting of horizontal rows and vertical columns.
  * `Field`: An individual named column within a table designed to store a specific type of data value.

---

### 15. What types of SQL subqueries do you know?
Subqueries can be Single-row (returns 1 value), Multi-row (returns multiple values), Multi-column, Nested, or Correlated (depends on outer query).

```sql
-- Single-row subquery returning exact highest order amount
SELECT order_id, total_amount 
FROM orders 
WHERE total_amount = (SELECT MAX(total_amount) FROM orders);

-- Multi-row subquery matching multiple customer IDs
SELECT order_id, customer_id 
FROM orders 
WHERE customer_id IN (SELECT id FROM priority_customers);
```

**Code Logic:**  
The first query finds the overall maximum order amount via `MAX()` and retrieves orders matching that exact single scalar value. The second query runs `SELECT id FROM priority_customers` returning an array of IDs, and uses `IN` to match all rows in `orders` tied to any of those IDs.

**Terms Used:**
  * `Subquery`: An inner SQL query embedded inside an outer statement.
  * `IN`: An operator filtering results against a list or multi-row subquery output.

---

### 16. What is a constraint, and why use constraints?
Constraints are rules enforced on table columns to maintain data accuracy, reliability, and integrity.

```sql
-- Enforce constraints ensuring valid data entry
CREATE TABLE bank_accounts (
    account_id INT NOT NULL,
    balance DECIMAL(10,2) CHECK (balance >= 0.00)
);
```

**Code Logic:**  
The engine creates the `bank_accounts` table enforcing two strict business rules: `account_id` cannot be assigned `NULL`, and `balance` insertions or updates will fail automatically if the balance drops below `0.00`.

**Terms Used:**
  * `Constraint`: Database-enforced validation rules restricting stored data values.
  * `CHECK`: A constraint verifying that stored column values satisfy a designated logical expression.

---

### 17. What SQL constraints do you know?
Common constraints include `NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `CHECK`, and `DEFAULT`.

```sql
-- Create table demonstrating multiple common column constraints
CREATE TABLE system_users (
    user_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE NOT NULL,
    user_status VARCHAR(20) DEFAULT 'PENDING'
);
```

**Code Logic:**  
`PRIMARY KEY` ensures `user_id` is unique and non-null. `UNIQUE NOT NULL` guarantees `email` cannot contain duplicates or empty null entries. `DEFAULT` automatically sets `user_status` to `'PENDING'` if no status value is passed during inserts.

**Terms Used:**
  * `PRIMARY KEY`: A constraint uniquely identifying each distinct record row in a table.
  * `UNIQUE`: A constraint preventing duplicate entries across rows within a target column.
  * `DEFAULT`: A constraint assigning an automatic fallback value when none is explicitly provided.

---

### 18. What types of joins do you know?
Standard joins include `INNER JOIN`, `LEFT (OUTER) JOIN`, `RIGHT (OUTER) JOIN`, `FULL (OUTER) JOIN`, `CROSS JOIN`, and `SELF JOIN`.

```sql
-- LEFT JOIN preserving all left table rows regardless of right table matches
SELECT c.customer_id, c.name, o.order_id 
FROM customers c 
LEFT JOIN orders o ON c.customer_id = o.customer_id;
```

**Code Logic:**  
The statement pulls every single record from `customers` (`LEFT` table). If a customer has corresponding rows in `orders` (`RIGHT` table), order fields are joined; if not, `o.order_id` outputs as `NULL` while retaining the customer row.

**Terms Used:**
  * `LEFT JOIN`: A join returning all rows from the left table and matching rows from the right table.
  * `NULL`: A special marker indicating absent, missing, or non-matching data entries.

---

### 19. What is a primary key in SQL?
A primary key is a column (or set of columns) that uniquely identifies each row in a table. It cannot contain `NULL` values and must be unique.

```sql
-- Define product_id as the primary lookup identifier
CREATE TABLE product_catalog (
    product_id INT PRIMARY KEY,
    product_sku VARCHAR(30)
);
```

**Code Logic:**  
The table creates `product_id` with a `PRIMARY KEY` constraint. The database engine automatically creates a unique index on `product_id` and blocks any future attempt to insert duplicate IDs or `NULL` values into this table.

**Terms Used:**
  * `PRIMARY KEY`: A table column constraint enforcing row uniqueness and non-null values.
  * `CREATE TABLE`: A DDL command creating a new table structure.

---

### 20. What is a unique key in SQL?
A unique key ensures all values in a column are distinct, but unlike a primary key, it allows a single `NULL` value (in most SQL engines).

```sql
-- Allow nullable entries while blocking duplicate social security numbers
CREATE TABLE employee_records (
    emp_id INT PRIMARY KEY,
    ssn_number VARCHAR(11) UNIQUE
);
```

**Code Logic:**  
The table establishes `emp_id` as the primary identifier, while `ssn_number` receives a `UNIQUE` constraint. This permits records to have distinct social security numbers, while still permitting `NULL` for temporary or unprovided values without throwing duplicate key errors.

**Terms Used:**
  * `UNIQUE`: A constraint ensuring no duplicate non-null data values exist within a field.
  * `PRIMARY KEY`: The main non-null unique identifier constraint for a database table.

---

### 21. What is a foreign key in SQL?
A foreign key is a column in one table that points to the `PRIMARY KEY` of another table to maintain referential integrity between them.

```sql
-- Establish a relational link from customer orders back to customer accounts
CREATE TABLE client_orders (
    order_id INT PRIMARY KEY,
    client_id INT FOREIGN KEY REFERENCES app_users(user_id)
);
```

**Code Logic:**  
The constraint `FOREIGN KEY REFERENCES app_users(user_id)` prevents orphan records by requiring every `client_id` entered into `client_orders` to already exist inside the primary key column `user_id` of the `app_users` table.

**Terms Used:**
  * `FOREIGN KEY`: A column link enforcing referential integrity back to an external primary key.
  * `PRIMARY KEY`: Target unique key column referenced by external foreign keys.

---

### 22. What is an SQL index?
An index is a data structure (usually a B-tree) created on table columns to drastically speed up data retrieval operations at the cost of slower writes.

```sql
-- Build an index on customer email addresses to accelerate search filters
CREATE INDEX idx_customers_email ON app_customers(email_address);
```

**Code Logic:**  
The engine constructs a background B-tree lookup structure (`idx_customers_email`) specifically referencing `email_address`. Subsequent queries using `WHERE email_address = 'user@test.com'` perform fast index seeks instead of scanning the full table sequentially.

**Terms Used:**
  * `CREATE INDEX`: A DDL command building a fast lookup structure for designated table columns.
  * `Index`: A database search structure created to speed up data query operations.

---

### 23. What types of indexes do you know?
Common index types include Clustered, Non-Clustered, Unique, Composite (multi-column), Full-Text, and B-Tree indexes.

```sql
-- Create a composite index spanning two lookup columns simultaneously
CREATE INDEX idx_emp_name_dept ON employees(last_name, department_id);
```

**Code Logic:**  
The system creates a composite index containing ordered keys for both `last_name` and `department_id`. Queries filtering on both columns simultaneously utilize this multi-column structure for optimized data retrieval.

**Terms Used:**
  * `Composite Index`: An index constructed using two or more combined table columns.
  * `CREATE INDEX`: Command creating secondary index structures on existing tables.

---

### 24. What is a schema?
A schema is a logical collection of database objects including tables, views, indexes, stored procedures, and constraints.

```sql
-- Instantiate a isolated database namespace and table inside it
CREATE SCHEMA finance_dept;
CREATE TABLE finance_dept.ledgers (ledger_id INT, amount DECIMAL(12,2));
```

**Code Logic:**  
The first statement creates a logical boundary namespace called `finance_dept`. The second statement creates the `ledgers` table explicitly inside this newly formed `finance_dept` schema container rather than the default public namespace.

**Terms Used:**
  * `CREATE SCHEMA`: A DDL command creating a logical namespace container for database objects.
  * `Schema`: A logical grouping structure organizing related database entities.

---

### 25. What is an SQL operator?
An operator is a reserved symbol or word used in SQL statements to perform operations like arithmetic, comparisons, or logical checks.

```sql
-- Filter items using comparison and logical operators
SELECT product_id, price 
FROM products 
WHERE price >= 100.00 AND in_stock = 1;
```

**Code Logic:**  
The database uses the `>=` comparison operator to check if `price` is at least `100.00`, and the `AND` logical operator to confirm that `in_stock` equals `1`. Only rows meeting both logical checks pass the filter.

**Terms Used:**
  * `Operator`: Symbols or keywords performing mathematical or conditional evaluations.
  * `AND`: A logical operator requiring both boolean conditions to evaluate as true.

---

### 26. What types of SQL operators do you know?
Operators include Arithmetic (`+`, `-`, `*`, `/`), Comparison (`=`, `!=`, `>`, `<`), Logical (`AND`, `OR`, `NOT`), and Special operators (`IN`, `BETWEEN`, `LIKE`, `IS NULL`).

```sql
-- Apply special range operator BETWEEN and set inclusion operator IN
SELECT item_name, price 
FROM inventory 
WHERE price BETWEEN 10.00 AND 50.00 AND category IN ('Tools', 'Hardware');
```

**Code Logic:**  
The `BETWEEN` operator evaluates whether `price` falls inclusively within the range of `10.00` to `50.00`. The `IN` operator verifies whether `category` matches any value in the provided text list (`'Tools'`, `'Hardware'`).

**Terms Used:**
  * `BETWEEN`: An operator checking if a value falls inclusively within a specified range.
  * `IN`: An operator checking if a value matches any item inside a explicit set list.

---

### 27. What is a clause?
A clause is a built-in component of an SQL statement used to filter, group, order, or manipulate data operations (e.g., `WHERE`, `GROUP BY`, `ORDER BY`).

```sql
-- Use clauses to filter, aggregate, and group transaction data
SELECT department_id, COUNT(*) AS total_staff 
FROM employees 
WHERE status = 'ACTIVE' 
GROUP BY department_id;
```

**Code Logic:**  
The `FROM` clause targets the table; `WHERE` restricts inputs to active rows; `GROUP BY` aggregates individual matching rows into department buckets; and `SELECT` outputs the department ID alongside row counts per group.

**Terms Used:**
  * `Clause`: Structural components of SQL queries performing designated statement operations.
  * `GROUP BY`: A clause grouping identical rows together for aggregate calculations.

---

### 28. What are some common statements used with the SELECT query?
Common companion clauses used with `SELECT` include `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, and `LIMIT` / `TOP`.

```sql
-- Filter, aggregate, filter groups, and sort result rows
SELECT dept_id, AVG(salary) AS avg_sal 
FROM employees 
WHERE status = 'FULLTIME' 
GROUP BY dept_id 
HAVING AVG(salary) > 60000.00 
ORDER BY avg_sal DESC;
```

**Code Logic:**  
1. `WHERE` filters individual `'FULLTIME'` rows.  
2. `GROUP BY` clusters rows by `dept_id`.  
3. `HAVING` filters out calculated department averages that are lower than or equal to `60000.00`.  
4. `ORDER BY` sorts final passing groups from highest average salary to lowest.

**Terms Used:**
  * `HAVING`: A clause filtering aggregated groups after `GROUP BY` calculations execute.
  * `ORDER BY`: A clause sorting returned query rows based on specified columns.

---

### 29. How do you create a table in SQL?
You create a table using the `CREATE TABLE` command followed by table name, column definitions, data types, and constraint specifications.

```sql
-- Construct a new staff table defining schema fields and data types
CREATE TABLE staff_members (
    staff_id INT PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    hire_date DATE
);
```

**Code Logic:**  
This statement executes DDL logic to create a persistent table `staff_members`. It assigns integer field `staff_id` as the primary key, text field `full_name` as non-nullable, and calendar field `hire_date` accepting `DATE` formats.

**Terms Used:**
  * `CREATE TABLE`: Command used to construct new structural tables in databases.
  * `VARCHAR`: Variable-length character string data type.

---

### 30. How to update a table?
You update a table using the `UPDATE` statement combined with `SET` to modify column values, usually filtered with a `WHERE` clause.

```sql
-- Raise salaries by 10 percent for active sales personnel
UPDATE staff_members 
SET salary = salary * 1.10 
WHERE department = 'Sales' AND status = 'ACTIVE';
```

**Code Logic:**  
The statement searches `staff_members` for rows where `department = 'Sales'` and `status = 'ACTIVE'`. For those specific matching rows, the `SET` clause re-calculates and overwrites existing values in `salary` with a 10% increase.

**Terms Used:**
  * `UPDATE`: Command used to modify column values in existing database rows.
  * `SET`: Sub-clause defining which target columns receive new computed values.

---

### 31. How to delete a table from a database?
You delete a table using the `DROP TABLE` command, which permanently removes the table structure and all data inside it.

```sql
-- Remove session table completely from current schema
DROP TABLE temporary_user_sessions;
```

**Code Logic:**  
The engine executes a DDL operation that instantly drops the object `temporary_user_sessions` from the catalog, releasing stored table rows, associated index structures, and structural schema definitions permanently.

**Terms Used:**
  * `DROP TABLE`: A DDL statement permanently destroying tables and their records.

---

### 32. How to get the count of records in a table?
You get the count using the `COUNT()` aggregate function inside a `SELECT` statement.

```sql
-- Calculate the total number of orders placed in 2026
SELECT COUNT(*) AS total_orders 
FROM customer_orders 
WHERE order_date >= '2026-01-01';
```

**Code Logic:**  
The `COUNT(*)` function counts every database record row from `customer_orders` that satisfies the `WHERE` filter condition, returning a single aggregate scalar total under the aliased label `total_orders`.

**Terms Used:**
  * `COUNT`: An aggregate function returning the numerical sum of rows matching criteria.
  * `AS`: Keyword assigning a descriptive alias name to function output columns.

---

### 33. How to sort records in a table?
You sort records using the `ORDER BY` clause, followed by column names and sorting direction (`ASC` for ascending, `DESC` for descending).

```sql
-- Return products ordered from highest price down to lowest price
SELECT product_name, price 
FROM products 
ORDER BY price DESC;
```

**Code Logic:**  
The query fetches `product_name` and `price` from `products`. Before outputting results, the `ORDER BY price DESC` clause instructs the query planner to sort output rows numerically in descending order.

**Terms Used:**
  * `ORDER BY`: A statement clause used to order query result rows.
  * `DESC`: Sorting parameter specifying high-to-low or Z-to-A ordering.

---

### 34. How to select all columns from a table?
You select all columns using the asterisk wildcard operator (`*`) inside a `SELECT` statement.

```sql
-- Retrieve all defined table columns for active customer accounts
SELECT * 
FROM customer_accounts 
WHERE status = 'ACTIVE';
```

**Code Logic:**  
The asterisk symbol (`*`) acts as an expansion wildcard. The database engine expands `*` to represent every column defined in `customer_accounts` without listing individual field names in the query text.

**Terms Used:**
  * `*`: Wildcard operator representing all available table columns.
  * `SELECT`: SQL command specifying which column fields to retrieve.

---

### 35. How to select common records from two tables?
You select common records by performing an `INNER JOIN` or by using the `INTERSECT` set operator.

```sql
-- Retrieve IDs present in both active employees and system admins
SELECT emp_id FROM active_employees 
INTERSECT 
SELECT emp_id FROM system_administrators;
```

**Code Logic:**  
The `INTERSECT` set operator processes two independent `SELECT` statements, compares the returned ID values, strips duplicate entries, and returns only unique `emp_id` records that exist in both result sets.

**Terms Used:**
  * `INTERSECT`: A set operator returning distinct rows shared between two queries.
  * `SELECT`: Query statement retrieving column values from tables.

---

### 36. What is the DISTINCT statement, and how do you use it?
Used with `SELECT` to eliminate duplicate rows and return only unique values.

```sql
-- Return unique locations represented across active vendor accounts
SELECT DISTINCT vendor_country 
FROM vendor_catalog 
WHERE vendor_status = 'ACTIVE';
```

**Code Logic:**  
The engine scans `vendor_catalog` for matching active rows, retrieves the string list of countries, strips out recurring duplicate country records, and returns a deduplicated list of unique countries.

**Terms Used:**
  * `DISTINCT`: A query modifier stripping duplicate values from returned results.

---

### 37. What are relationships? Give some examples.
Relationships are logical links established between database tables via primary and foreign keys. Examples: One-to-One (1:1), One-to-Many (1:N), and Many-to-Many (M:N).

```sql
-- One-to-Many link: One customer can own multiple order records
CREATE TABLE order_invoices (
    invoice_id INT PRIMARY KEY,
    customer_id INT REFERENCES customer_accounts(id)
);
```

**Code Logic:**  
This code models a 1:N relationship. `customer_accounts` stores individual customers once, while `order_invoices` uses foreign key `customer_id` to allow many separate invoice rows to link back to the same primary customer account.

**Terms Used:**
  * `PRIMARY KEY`: Unique row identifier inside parent tables.
  * `REFERENCES`: Keyword establishing foreign key constraints back to primary tables.

---

### 38. What is a NULL value? How is it different from zero or a blank space?
`NULL` represents a total absence of a value or unknown state. It is not equivalent to zero (0) or empty text (`''`).

```sql
-- Retrieve users whose contact phone numbers have not been entered
SELECT user_id, username 
FROM app_users 
WHERE phone_number IS NULL;
```

**Code Logic:**  
Because `NULL` represents missing data rather than a literal scalar value, standard equality comparison operators (`= NULL`) fail. The query uses `IS NULL` to locate rows where `phone_number` contains an unallocated state.

**Terms Used:**
  * `NULL`: SQL state indicator representing missing, missing-or-unknown data.
  * `IS NULL`: Special operator evaluating whether a field value is in an unallocated `NULL` state.

---

### 39. What is the difference between SQL and NoSQL?
SQL databases are relational, table-based, structured, and use fixed schemas. NoSQL databases are non-relational, document/key-value based, unstructured, and schema-flexible.

```sql
-- Structured relational query dependent on fixed pre-defined schemas
SELECT id, username, email 
FROM relational_users 
WHERE user_age >= 21;
```

**Code Logic:**  
The relational query relies on an explicit table definition (`relational_users`) containing defined data types and named columns (`id`, `username`, `email`, `user_age`), projecting matching structured records.

**Terms Used:**
  * `Relational`: Database design model organizing structured data into linked tables.
  * `Schema`: Structural blueprint dictating defined column fields and strict data types.

---

### 40. What are some common challenges when working with SQL databases?
Challenges include slow query performance on massive datasets, handling database locks/deadlocks, schema migration risk, and scaling hardware vertically.

```sql
-- Explain execution details to identify slow table scan issues
EXPLAIN ANALYZE 
SELECT * FROM tracking_logs WHERE search_code = 'ERR_404';
```

**Code Logic:**  
`EXPLAIN ANALYZE` commands the database planner to execute the query while capturing operational metrics, showing developers whether the search used an index lookup or suffered performance degradation from a full table scan.

**Terms Used:**
  * `EXPLAIN ANALYZE`: Performance profiling tool returning physical engine execution plans.

---

## Section 4: Intermediate SQL Questions

### 41. What is a Common Table Expression (CTE)?
A Common Table Expression (CTE) is a temporary, named result set defined using the `WITH` keyword that simplifies complex multi-step queries and can be referenced within the main query.

```sql
-- Define CTE to isolate high value transactions for main query processing
WITH HighValueOrders AS (
    SELECT order_id, customer_id, total_amount 
    FROM sales_orders 
    WHERE total_amount > 1000.00
)
SELECT customer_id, COUNT(*) AS big_orders 
FROM HighValueOrders 
GROUP BY customer_id;
```

**Code Logic:**  
The `WITH` block instantiates a temporary named query set `HighValueOrders` filtering orders over `$1000.00`. The main query immediately references this CTE like a temporary table, grouping results to count big orders per customer.

**Terms Used:**
  * `WITH`: Keyword introducing Common Table Expressions (CTEs).
  * `CTE`: Common Table Expression; a temporary named query result block.

---

### 42. What are window functions, and how do they differ from aggregate functions?
Window functions perform calculations across a set of related table rows without collapsing rows together like `GROUP BY` aggregate functions do.

```sql
-- Compute department average alongside original uncollapsed employee rows
SELECT full_name, dept_id, salary,
       AVG(salary) OVER (PARTITION BY dept_id) AS dept_avg_salary
FROM company_employees;
```

**Code Logic:**  
Unlike `GROUP BY`, which would collapse output rows, `AVG(salary) OVER (PARTITION BY dept_id)` calculates average department salary across partitioned groups while preserving every individual employee record row in the output.

**Terms Used:**
  * `OVER`: Clause defining the operational window frame for analytic functions.
  * `PARTITION BY`: Sub-clause dividing dataset rows into window computation groups.

---

### 43. What is the difference between RANK(), DENSE_RANK(), and ROW_NUMBER()?
All assignment functions rank rows over an ordered partition: `ROW_NUMBER()` assigns strict sequential numbers; `RANK()` leaves gaps after tied values; `DENSE_RANK()` leaves no gaps after tied values.

```sql
-- Rank tied competition scores using three distinct ranking approaches
SELECT student_name, score,
       ROW_NUMBER() OVER (ORDER BY score DESC) AS row_num,
       RANK()       OVER (ORDER BY score DESC) AS rnk,
       DENSE_RANK() OVER (ORDER BY score DESC) AS dense_rnk
FROM exam_scores;
```

**Code Logic:**  
For tied scores (e.g., scores `95, 95, 90`), `ROW_NUMBER()` outputs sequential `1, 2, 3`. `RANK()` outputs `1, 1, 3` (skipping 2 due to ties). `DENSE_RANK()` outputs `1, 1, 2` (preserving continuous dense sequential ranks).

**Terms Used:**
  * `ROW_NUMBER`: Analytic function outputting unique sequential row integers.
  * `RANK`: Ranking function skipping numbers after duplicate values.
  * `DENSE_RANK`: Ranking function maintaining continuous integer sequences without gaps.

---

### 44. What is a function in SQL?
A routine that accepts input parameters, performs calculations or transformations, and returns a result value or table.

```sql
-- Convert names to uppercase and round monetary output values
SELECT UPPER(client_name), ROUND(account_balance, 1) 
FROM client_accounts;
```

**Code Logic:**  
The query applies two built-in scalar functions on every row: `UPPER()` transforms text values in `client_name` to uppercase, while `ROUND()` truncates `account_balance` values to a single decimal digit.

**Terms Used:**
  * `UPPER`: Scalar string function converting text characters to uppercase.
  * `ROUND`: Math function rounding numeric inputs to designated decimal precision.

---

### 45. What types of SQL functions do you know?
SQL functions split broadly into Aggregate Functions (multi-row inputs producing single output) and Scalar Functions (single-row input producing single-row output).

```sql
-- Apply scalar function LENGTH inside aggregate calculation MAX
SELECT MAX(LENGTH(product_title)) AS longest_title_length 
FROM product_catalog;
```

**Code Logic:**  
`LENGTH()` is a scalar function that evaluates character lengths row-by-row. `MAX()` is an aggregate function that scans those resulting length numbers across all rows and returns the single highest numerical integer.

**Terms Used:**
  * `LENGTH`: Scalar function returning the character count of a text string.
  * `MAX`: Aggregate function returning the largest scalar value across rows.

---

### 46. What SQL aggregate functions do you know?
Key aggregate functions include `COUNT()`, `SUM()`, `AVG()`, `MIN()`, and `MAX()`.

```sql
-- Aggregate branch statistics for financial analysis
SELECT branch_id, COUNT(*) AS staff_count, AVG(salary) AS avg_salary, MAX(salary) AS peak_salary
FROM branch_staff
GROUP BY branch_id;
```

**Code Logic:**  
The query groups staff rows by `branch_id` and runs four aggregate calculations per branch: counting total group rows, averaging salary values, and evaluating the highest salary present within that branch bucket.

**Terms Used:**
  * `COUNT`: Aggregate function counting total grouped table rows.
  * `AVG`: Aggregate function returning arithmetic mean numerical values.
  * `MAX`: Aggregate function finding the single maximum value in a group.

---

### 47. What SQL scalar functions do you know?
Functions operating on individual items. Includes String (`UPPER`, `LOWER`, `CONCAT`, `SUBSTRING`), Numeric (`ROUND`, `ABS`), Date (`NOW`, `DATEDIFF`), and System functions.

```sql
-- Combine text strings and round prices on individual rows
SELECT CONCAT(first_name, ' ', last_name) AS full_name, ROUND(unit_price, 0) AS rounded_price 
FROM item_sales;
```

**Code Logic:**  
For each row, `CONCAT()` joins the `first_name` string, a space, and `last_name` into a single text output, while `ROUND(unit_price, 0)` converts decimal monetary values into rounded whole integers.

**Terms Used:**
  * `CONCAT`: Scalar function merging string inputs together.
  * `ROUND`: Scalar function rounding numeric fields to specified positions.

---

### 48. What are case manipulation functions in SQL?
String functions used to alter character casing in text data: `LOWER()`, `UPPER()`, and `INITCAP()` (capitalizes first letter of each word).

```sql
-- Standardize search formatting and casing output
SELECT LOWER(user_email) AS clean_email, UPPER(country_code) AS formatted_country 
FROM user_profiles;
```

**Code Logic:**  
`LOWER()` normalizes variable inputs in `user_email` to lowercase characters (e.g., `'User@Test.COM'` becomes `'user@test.com'`), while `UPPER()` converts text characters in `country_code` to uppercase.

**Terms Used:**
  * `LOWER`: Case manipulation function converting string characters to lower case.
  * `UPPER`: Case manipulation function converting string characters to upper case.

---

### 49. What are character manipulation functions in SQL?
Functions used to inspect or edit text fields, such as `CONCAT()`, `SUBSTR()` / `SUBSTRING()`, `LENGTH()`, `TRIM()`, `REPLACE()`, and `INSTR()`.

```sql
-- Extract substring area codes and count character length
SELECT SUBSTRING(phone_number, 1, 3) AS area_code, LENGTH(user_notes) AS note_length 
FROM user_contacts;
```

**Code Logic:**  
`SUBSTRING(phone_number, 1, 3)` extracts a 3-character text string starting at index position 1. `LENGTH(user_notes)` counts the total number of characters contained inside the `user_notes` text field.

**Terms Used:**
  * `SUBSTRING`: Character function pulling specific sub-segments from text.
  * `LENGTH`: Character function measuring the scalar length of text strings.

---

### 50. What is the difference between local and global variables?
Local variables exist only within the specific block, stored procedure, or batch where defined. Global variables exist across session scope or system-wide.

```sql
-- SQL Server syntax declaring and assigning a local batch variable
DECLARE @TargetDeptID INT = 5;
SELECT * FROM departmental_staff WHERE dept_id = @TargetDeptID;
```

**Code Logic:**  
The script uses `DECLARE` to allocate a temporary local batch variable named `@TargetDeptID` set to integer `5`. The subsequent `SELECT` query references `@TargetDeptID` directly within its `WHERE` filter condition.

**Terms Used:**
  * `DECLARE`: Command allocating local procedural script variables.
  * `@`: Symbol prefix denoting local variable definitions in SQL Server environments.

---

### 51. What is the difference between SQL and PL/SQL?
SQL is a declarative database query language. PL/SQL (Procedural Language/SQL) is Oracle’s proprietary extension adding procedural elements like loops, branches, and variables.

```sql
-- PL/SQL block wrapping transactional SQL inside procedural logic
BEGIN
    UPDATE client_balances SET balance = balance - 100.00 WHERE account_id = 501;
    COMMIT;
END;
```

**Code Logic:**  
The code uses a PL/SQL procedural wrapper block (`BEGIN ... END;`) to safely execute an `UPDATE` operation reducing account balances and explicitly commit the transaction to the database engine.

**Terms Used:**
  * `BEGIN / END`: Procedural block wrapper scope boundaries in PL/SQL.
  * `COMMIT`: Transactional command saving physical modifications permanently.

---

### 52. What is the difference between LEFT JOIN and LEFT OUTER JOIN?
There is zero functional difference. `LEFT JOIN` is shorthand syntax for `LEFT OUTER JOIN`.

```sql
-- Both query syntaxes yield identical execution plans
SELECT a.id, b.info FROM TableA a LEFT JOIN TableB b ON a.id = b.id;
SELECT a.id, b.info FROM TableA a LEFT OUTER JOIN TableB b ON a.id = b.id;
```

**Code Logic:**  
Both queries instruct the query parser to perform the same outer join operation, returning all rows from `TableA` regardless of whether matching primary/foreign key rows are present inside `TableB`.

**Terms Used:**
  * `LEFT JOIN`: Shorthand notation for left outer join commands.
  * `LEFT OUTER JOIN`: Full syntactic phrase performing left outer joins.

---

### 53. What is indexing in SQL, and how does it improve performance?
Indexing creates a sorted lookup data structure (e.g., B-Tree) mapping index keys to physical row addresses, avoiding slow full-table scans during search operations.

```sql
-- Accelerate customer lookup speed on order searches
CREATE INDEX idx_orders_customer ON customer_orders(customer_id);
```

**Code Logic:**  
This DDL statement constructs a secondary index structure on `customer_id`. When queries search for `WHERE customer_id = 88`, the engine jumps straight to target row locations using the index seek rather than reading every page of `customer_orders`.

**Terms Used:**
  * `CREATE INDEX`: Command constructing fast index lookup structures.
  * `Index Seek`: Quick traversal of index B-trees to target specific records directly.

---

### 54. What is a stored procedure, and how is it different from a function?
A stored procedure is a compiled group of reusable SQL statements that performs complex workflows and may alter database state. Unlike functions, procedures don't have to return a value and cannot be called directly within a `SELECT` statement.

```sql
-- Create a stored procedure modifying salary records safely
CREATE PROCEDURE PromoteEmployee(IN emp_id_param INT, IN raise_amount DECIMAL(10,2))
BEGIN
    UPDATE company_staff 
    SET salary = salary + raise_amount 
    WHERE emp_id = emp_id_param;
END;
```

**Code Logic:**  
This code compiles a database procedure `PromoteEmployee` taking two input parameters. When called, it executes the enclosed `UPDATE` statement to increase the specified employee's salary in physical table storage.

**Terms Used:**
  * `CREATE PROCEDURE`: Command building reusable database procedural routines.
  * `UPDATE`: Data manipulation language command modifying column data.

---

### 55. What is the default data ordering with the ORDER BY statement, and how do you change it?
The default sort order is Ascending (`ASC`). You change it to Descending by explicitly adding `DESC`.

```sql
-- Explicitly override default ascending sorting behavior
SELECT item_name, retail_price 
FROM inventory_items 
ORDER BY retail_price DESC;
```

**Code Logic:**  
By adding `DESC` after `retail_price` within the `ORDER BY` clause, the database query engine overrides its default ascending order (`ASC`) and sorts returned rows starting from highest price down to lowest price.

**Terms Used:**
  * `ORDER BY`: Clause dictating query output ordering rules.
  * `DESC`: Keyword forcing reverse/descending order sorting behavior.

---

### 56. What are SQL set operators?
Set operators combine results from two distinct `SELECT` queries into a single result set. Includes `UNION`, `UNION ALL`, `INTERSECT`, and `EXCEPT` / `MINUS`.

```sql
-- Merge full list of contact names including internal duplicates
SELECT contact_name FROM staff_directory
UNION ALL
SELECT contact_name FROM contractor_directory;
```

**Code Logic:**  
The `UNION ALL` set operator merges all output records from the first query with output records from the second query, preserving all duplicates without performing expensive deduplication passes.

**Terms Used:**
  * `UNION ALL`: Set operator combining query result rows including duplicates.
  * `SELECT`: Query statement extracting column data from target tables.

---

### 57. What operator is used in the query for pattern matching?
The `LIKE` operator, using wildcard characters `%` (matches multiple characters) and `_` (matches exactly one character).

```sql
-- Search for emails starting with 'j' and ending with '@gmail.com'
SELECT user_id, email_address 
FROM app_users 
WHERE email_address LIKE 'j%@gmail.com';
```

**Code Logic:**  
The `LIKE` pattern `'j%@gmail.com'` checks string entries in `email_address`. The `%` wildcard matches any intermediate string sequence between an initial `'j'` character and the required `'@gmail.com'` ending.

**Terms Used:**
  * `LIKE`: Pattern matching operator used inside conditional clauses.
  * `%`: Multi-character wildcard matching sequences of zero or more characters.

---

### 58. What is the difference between a primary key and a unique key in SQL?
A primary key uniquely identifies rows, prohibits `NULL` values completely, and a table allows only one primary key. A unique key prevents duplicates, usually allows one `NULL`, and a table can have multiple unique keys.

```sql
-- Primary key identifies row, while unique keys protect unique email/SSN
CREATE TABLE user_registry (
    user_id INT PRIMARY KEY,
    email_address VARCHAR(100) UNIQUE,
    ssn_code VARCHAR(11) UNIQUE
);
```

**Code Logic:**  
`user_id` is defined as the table's single `PRIMARY KEY`. The schema adds two independent `UNIQUE` key constraints on `email_address` and `ssn_code` to allow multiple distinct fields to enforce duplicate checks independently.

**Terms Used:**
  * `PRIMARY KEY`: Primary unique table identifier blocking null values.
  * `UNIQUE`: Secondary column constraint enforcing uniqueness across populated rows.

---

### 59. What is a SQL composite primary key?
A primary key consisting of two or more combined columns to uniquely identify each row in a table.

```sql
-- Define multi-column composite key bridging orders and items
CREATE TABLE order_line_items (
    order_id INT,
    item_id INT,
    quantity_ordered INT,
    PRIMARY KEY (order_id, item_id)
);
```

**Code Logic:**  
The table specifies `PRIMARY KEY (order_id, item_id)`. Neither column alone needs to be unique, but every combined pair (e.g., order `1001`, item `5`) must be strictly unique across the entire table.

**Terms Used:**
  * `PRIMARY KEY`: Multi-column definition block creating composite primary identifiers.
  * `Composite Key`: Key structure constructed using multiple individual columns.

---

### 60. What is the typical order of SQL clauses in a SELECT statement?
Written order: `SELECT` -> `FROM` -> `JOIN` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `ORDER BY` -> `LIMIT`.

```sql
-- Complete SELECT query exhibiting proper syntactic clause sequence
SELECT department_id, COUNT(*) AS staff_count
FROM company_staff
WHERE hire_date >= '2020-01-01'
GROUP BY department_id
HAVING COUNT(*) > 5
ORDER BY staff_count DESC
LIMIT 3;
```

**Code Logic:**  
The query follows strict SQL clause structure: selecting output fields, specifying source table, filtering individual rows, grouping records, filtering aggregate outputs, ordering output, and capping total output rows to 3.

**Terms Used:**
  * `HAVING`: Clause filtering aggregated groups after `GROUP BY`.
  * `LIMIT`: Clause capping the maximum number of rows returned in query outputs.

---

### 61. In which order does the interpreter execute the common statements in the SELECT query?
Execution order: 1. `FROM` / `JOIN` -> 2. `WHERE` -> 3. `GROUP BY` -> 4. `HAVING` -> 5. `SELECT` -> 6. `DISTINCT` -> 7. `ORDER BY` -> 8. `LIMIT`.

```sql
-- Logical processing evaluates FROM and WHERE long before SELECT runs
SELECT dept_id, AVG(salary) AS avg_sal
FROM employees
WHERE status = 'ACTIVE'
GROUP BY dept_id
HAVING AVG(salary) > 50000.00;
```

**Code Logic:**  
The engine loads tables in `FROM`, evaluates row filters in `WHERE`, creates group sets in `GROUP BY`, filters calculated groups in `HAVING`, and projects target alias outputs (`avg_sal`) during the `SELECT` phase.

**Terms Used:**
  * `FROM`: Initial execution step defining source data tables.
  * `SELECT`: Late-stage execution step calculating and projecting final output columns.

---

### 62. What is a view in SQL?
A view is a virtual table based on the stored result of a pre-written `SELECT` statement. It doesn't store physical data itself.

```sql
-- Create virtual view wrapper exposing active user contact details
CREATE VIEW v_active_user_contacts AS
SELECT id, username, email_address 
FROM user_accounts 
WHERE account_status = 'ACTIVE';

-- Query the created virtual view
SELECT * FROM v_active_user_contacts;
```

**Code Logic:**  
`CREATE VIEW` stores the query logic under the named object `v_active_user_contacts`. When queried, the engine runs the underlying `SELECT` statement dynamically against base table `user_accounts`.

**Terms Used:**
  * `CREATE VIEW`: DDL command defining virtual table query wrappers.
  * `View`: A saved virtual table representation generated from dynamic underlying queries.

---

### 63. Can we create a view based on another view in SQL?
Yes, this is known as nested views. The new view queries the existing view as its data source.

```sql
-- Create a nested view filtering an existing active users view
CREATE VIEW v_us_active_contacts AS
SELECT id, username, email_address 
FROM v_active_user_contacts 
WHERE country_code = 'US';
```

**Code Logic:**  
The statement builds a secondary virtual view `v_us_active_contacts` using the pre-existing view `v_active_user_contacts` as its data source, appending an extra conditional filter `country_code = 'US'`.

**Terms Used:**
  * `CREATE VIEW`: DDL keyword creating nested virtual table definitions.
  * `Nested View`: A view object created by selecting data directly from an existing view.

---

### 64. Can we still use a view if the original table is deleted?
No. When the base table is dropped, the view becomes invalid/broken and queries against it will throw errors.

```sql
-- Drop base physical table
DROP TABLE user_accounts;

-- Querying view now throws an invalid object database error
SELECT * FROM v_active_user_contacts;
```

**Code Logic:**  
Since views do not duplicate physical table storage on disk, dropping `user_accounts` destroys underlying source fields. Calling `SELECT * FROM v_active_user_contacts` fails because referenced table objects no longer exist.

**Terms Used:**
  * `DROP TABLE`: DDL command permanently deleting base physical tables.
  * `View`: A virtual query wrapper entirely dependent on base table existence.

---

### 65. What types of SQL relationships do you know?
One-to-One (1:1), One-to-Many (1:N or Many-to-One), and Many-to-Many (M:N).

```sql
-- Implement Many-to-Many relationship using an explicit junction table
CREATE TABLE student_course_bridge (
    student_id INT REFERENCES students(id),
    course_id INT REFERENCES courses(id),
    PRIMARY KEY (student_id, course_id)
);
```

**Code Logic:**  
Because one student can enroll in multiple courses and one course contains multiple students, `student_course_bridge` acts as a junction table storing pair keys that map M:N relationships cleanly.

**Terms Used:**
  * `Junction Table`: A bridge table resolving Many-to-Many entity relationships.
  * `REFERENCES`: Foreign key clause pointing back to primary record tables.

---

### 66. What are the possible values of a BOOLEAN data field?
Three possibilities under SQL's 3-valued logic: `TRUE`, `FALSE`, and `NULL` (unknown).

```sql
-- Create task record table featuring default false boolean field
CREATE TABLE project_tasks (
    task_id INT PRIMARY KEY,
    is_completed BOOLEAN DEFAULT FALSE
);
```

**Code Logic:**  
The statement configures column `is_completed` to accept standard boolean input values (`TRUE`, `FALSE`). If a record is inserted without assigning a status value, the `DEFAULT` clause populates it with `FALSE`.

**Terms Used:**
  * `BOOLEAN`: Data type accepting binary logical values (`TRUE`, `FALSE`, `NULL`).
  * `DEFAULT`: Automatic fallback constraint assigned when input values are omitted.

---

### 67. What is normalization in SQL?
The process of organizing data tables to minimize redundant data and eliminate data anomalies during inserts, updates, and deletes (1NF, 2NF, 3NF, BCNF).

```sql
-- Normalized architecture separating user entities from order entities
CREATE TABLE users (user_id INT PRIMARY KEY, name VARCHAR(50));
CREATE TABLE orders (order_id INT PRIMARY KEY, user_id INT REFERENCES users(user_id));
```

**Code Logic:**  
Instead of duplicating user names inside every order record row, normalization isolates user details inside the `users` table, referencing them inside `orders` via foreign key `user_id` to eliminate redundant data.

**Terms Used:**
  * `Normalization`: Database design technique organizing fields to minimize data redundancy.
  * `PRIMARY KEY`: Unique identifier key establishing relational entity boundaries.

---

### 68. What is denormalization in SQL?
The deliberate practice of adding redundant data back into normalized tables to reduce costly `JOIN` operations and optimize query read performance.

```sql
-- Store customer_name directly on orders table to speed up read queries
CREATE TABLE fast_orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    customer_name VARCHAR(100) -- Redundant column added to bypass JOINs
);
```

**Code Logic:**  
By duplicating `customer_name` directly into `fast_orders`, select queries can output full order summary reports directly from one table, completely bypassing the CPU and memory cost of performing SQL `JOIN` operations.

**Terms Used:**
  * `Denormalization`: Deliberately introducing redundant data to optimize read performance.
  * `JOIN`: Relational operation bypassed through denormalized schema strategies.

---

### 69. What is the difference between renaming a column and giving an alias to it?
Renaming physically alters column names permanently in table metadata (`ALTER TABLE`). An alias assigns a temporary name for query output formatting without modifying table schema.

```sql
-- Permanent schema modification altering database metadata
ALTER TABLE user_accounts RENAME COLUMN fname TO first_name;

-- Temporary query projection alias
SELECT fname AS first_name FROM user_accounts;
```

**Code Logic:**  
The `ALTER TABLE` statement modifies the permanent catalog schema structure. In contrast, the `SELECT ... AS` query executes temporary formatting changes applying only to the active query output result set.

**Terms Used:**
  * `ALTER TABLE`: DDL command executing permanent database schema modifications.
  * `AS`: Keyword assigning temporary execution output alias headers.

---

### 70. What is the difference between nested and correlated subqueries?
A nested subquery runs independently of the outer query once. A correlated subquery references columns from the outer query, running repeatedly once for every row processed by the outer query.

```sql
-- Correlated Subquery referencing parent outer row e1.dept_id
SELECT e1.name, e1.dept_id, e1.salary 
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary) 
    FROM employees e2 
    WHERE e2.dept_id = e1.dept_id
);
```

**Code Logic:**  
For each row evaluated by outer query `e1`, the inner subquery executes using that specific row's `e1.dept_id`, dynamically calculating localized department average salaries to evaluate individual employee row qualifications.

**Terms Used:**
  * `Correlated Subquery`: Subquery evaluated repeatedly using values provided by parent outer queries.
  * `AVG`: Aggregate function computing numeric dataset averages.

---

### 71. What is the difference between clustered and non-clustered indexes?
A clustered index determines the physical storage order of table data (only 1 per table). A non-clustered index creates a separate lookup structure with pointers to actual rows (multiple allowed per table).

```sql
-- Primary key forms clustered physical order; secondary index builds non-clustered lookup
CREATE TABLE books_catalog (book_id INT PRIMARY KEY, title VARCHAR(100));
CREATE INDEX idx_books_title ON books_catalog(title);
```

**Code Logic:**  
The `PRIMARY KEY` organizes how rows in `books_catalog` are physically ordered on disk. `CREATE INDEX` creates a separate non-clustered B-tree containing sorted `title` keys that point back to physical primary key row locations.

**Terms Used:**
  * `Clustered Index`: Index dictating physical table record layout on disk storage.
  * `Non-Clustered Index`: Secondary lookup structure holding pointers back to physical rows.

---

### 72. What is the CASE() function?
SQL’s conditional expression logic equivalent to IF-THEN-ELSE statements inside queries.

```sql
-- Evaluate price tiers conditionally per row output
SELECT item_name,
       CASE 
           WHEN price >= 100.00 THEN 'PREMIUM'
           WHEN price >= 50.00 THEN 'MID-RANGE'
           ELSE 'BUDGET'
       END AS price_category
FROM inventory_items;
```

**Code Logic:**  
The `CASE` statement inspects `price` for every output row. It returns `'PREMIUM'` if price is at least `100.00`, `'MID-RANGE'` if price is at least `50.00`, and `'BUDGET'` for all remaining items under calculated column label `price_category`.

**Terms Used:**
  * `CASE`: Conditional evaluation statement returning specified values based on logical checks.
  * `WHEN / THEN`: Conditional block pairing branch checks with result outputs.

---

### 73. What is the difference between the DELETE and TRUNCATE statements?
`DELETE` is a DML command that removes specific or all rows one by one, logs row deletes, and supports `WHERE` clauses. `TRUNCATE` is a DDL command that deallocates entire table storage pages quickly, cannot use `WHERE`, and is much faster.

```sql
-- DELETE: Target specific rows with logging support
DELETE FROM system_logs WHERE log_date < '2025-01-01';

-- TRUNCATE: Instantly clear all table data pages without row filters
TRUNCATE TABLE temp_processing_logs;
```

**Code Logic:**  
`DELETE` scans `system_logs` and removes individual matching rows while maintaining transaction logs. `TRUNCATE` bypasses row scanning entirely, instantly resetting table storage allocation pages for `temp_processing_logs`.

**Terms Used:**
  * `DELETE`: Filterable DML command removing specific target table rows.
  * `TRUNCATE`: Fast DDL command deallocating table storage data pages.

---

### 74. What is the difference between the DROP and TRUNCATE statements?
`TRUNCATE` deletes all data rows from a table while preserving the table schema structure. `DROP` deletes both data rows AND the table schema permanently from the database.

```sql
-- Table structure survives, contents empty
TRUNCATE TABLE staging_data;

-- Both structure and contents erased permanently
DROP TABLE staging_data;
```

**Code Logic:**  
After running `TRUNCATE TABLE`, developers can immediately execute `INSERT` queries into `staging_data` because column definitions survive. After running `DROP TABLE`, `staging_data` ceases to exist in catalog metadata.

**Terms Used:**
  * `TRUNCATE`: DDL command clearing data records while keeping table structure intact.
  * `DROP`: DDL command permanently erasing table definitions and contents completely.

---

### 75. What is the difference between the HAVING and WHERE statements?
`WHERE` filters individual input rows before aggregation occurs. `HAVING` filters grouped aggregate results after `GROUP BY` execution.

```sql
-- Apply WHERE row filtering before grouping, and HAVING aggregate filtering after grouping
SELECT dept_id, COUNT(*) AS staff_count
FROM company_employees
WHERE employment_status = 'ACTIVE'
GROUP BY dept_id
HAVING COUNT(*) > 10;
```

**Code Logic:**  
1. `WHERE` filters out non-active employee rows before calculations start.  
2. `GROUP BY` aggregates remaining active employees into department groups.  
3. `HAVING` filters aggregate group outputs, returning only departments containing over 10 active staff members.

**Terms Used:**
  * `WHERE`: Clause filtering row records before aggregation occurs.
  * `HAVING`: Clause filtering calculated aggregate groups after `GROUP BY`.

---

### 76. How do you add a record to a table?
You add a record using the `INSERT INTO` statement specifying target table, columns, and values.

```sql
-- Insert a new user record with explicit column target values
INSERT INTO user_profiles (user_id, username, email) 
VALUES (50, 'm_sefidgar', 'user@example.com');
```

**Code Logic:**  
The `INSERT INTO` command identifies target table `user_profiles` and target column fields (`user_id`, `username`, `email`). The `VALUES` clause supplies matching ordered data tuple literal values (`50`, `'m_sefidgar'`, `'user@example.com'`).

**Terms Used:**
  * `INSERT INTO`: Command used to write new data rows into target tables.
  * `VALUES`: Sub-clause defining literal data row values to insert.

---

### 77. How do you delete a record from a table?
You delete a record using the `DELETE FROM` statement paired with a matching `WHERE` clause condition.

```sql
-- Remove a specific user record based on primary key identifier
DELETE FROM user_profiles 
WHERE user_id = 50;
```

**Code Logic:**  
The engine inspects `user_profiles`, locates the row matching primary key filter `WHERE user_id = 50`, and deletes that individual row entry from physical table storage.

**Terms Used:**
  * `DELETE FROM`: Statement command used to remove specified data rows.
  * `WHERE`: Filtering clause ensuring only matching target rows are removed.

---

### 78. How do you add a column to a table?
You add a column using the `ALTER TABLE` statement combined with the `ADD` clause.

```sql
-- Expand existing schema structure to store user phone details
ALTER TABLE user_profiles 
ADD phone_number VARCHAR(20);
```

**Code Logic:**  
This DDL statement updates catalog metadata for `user_profiles`, appending a new field column `phone_number` formatted as `VARCHAR(20)` initialized with `NULL` values across existing rows.

**Terms Used:**
  * `ALTER TABLE`: Command used to modify structural table metadata definitions.
  * `ADD`: Sub-clause appending new field columns to existing table schemas.

---

### 79. How do you rename a column of a table?
You rename a column using `ALTER TABLE` with `RENAME COLUMN ... TO ...`.

```sql
-- Rename existing column field in table metadata permanently
ALTER TABLE user_profiles 
RENAME COLUMN phone_number TO contact_phone;
```

**Code Logic:**  
The database engine updates its internal data dictionary for `user_profiles`, permanently modifying the column identifier `phone_number` to its replacement name `contact_phone` without altering underlying row data values.

**Terms Used:**
  * `ALTER TABLE`: Structural statement modifying catalog schema definitions.
  * `RENAME COLUMN`: Clause specifying target field name changes.

---

### 80. How do you delete a column from a table?
You delete a column using `ALTER TABLE` combined with `DROP COLUMN`.

```sql
-- Permanently remove specified column field from table structure
ALTER TABLE user_profiles 
DROP COLUMN contact_phone;
```

**Code Logic:**  
The DDL statement alters `user_profiles` schema metadata by stripping the field column `contact_phone` and removing stored data values associated with that column across all existing table rows.

**Terms Used:**
  * `ALTER TABLE`: DDL command modifying table schema structures.
  * `DROP COLUMN`: Sub-clause removing defined field columns permanently.

---

### 81. How do you select all even or all odd records in a table?
You select them using the modulo operator (`%` or `MOD()`) on an ID or row number field.

```sql
-- Fetch records with even primary key IDs
SELECT user_id, username 
FROM app_users 
WHERE user_id % 2 = 0;

-- Fetch records with odd primary key IDs
SELECT user_id, username 
FROM app_users 
WHERE user_id % 2 != 0;
```

**Code Logic:**  
The modulo operator `%` calculates the arithmetic remainder of dividing `user_id` by `2`. If `user_id % 2 = 0`, the integer ID is even; if remainder is non-zero (`!= 0`), the ID is odd.

**Terms Used:**
  * `%`: Modulo arithmetic operator returning division remainders.
  * `WHERE`: Filtering clause evaluating modulo conditional outputs.

---

### 82. How to prevent duplicate records when making a query?
You prevent them by using the `DISTINCT` keyword or grouping results using `GROUP BY`.

```sql
-- Deduplicate output list of customer locations
SELECT DISTINCT location_city 
FROM customer_profiles;
```

**Code Logic:**  
The engine executes `SELECT DISTINCT`, scanning `customer_profiles` for `location_city` values, stripping recurring duplicates, and outputting a single unique representative instance for every discovered city string.

**Terms Used:**
  * `DISTINCT`: Keyword modifier eliminating duplicate rows from output sets.

---

### 83. How do you insert many rows in a table?
You insert many rows by combining multiple comma-separated value tuples into a single `INSERT INTO` statement.

```sql
-- Execute bulk insertion passing multiple value tuples simultaneously
INSERT INTO product_categories (category_id, category_name) VALUES 
(1, 'Electronics'),
(2, 'Home Appliances'),
(3, 'Office Supplies');
```

**Code Logic:**  
Instead of sending three separate SQL queries, this single statement passes three ordered row value tuples directly inside one `INSERT INTO` execution, improving batch transaction speed.

**Terms Used:**
  * `INSERT INTO`: Command specifying target write tables.
  * `VALUES`: Sub-clause wrapping batch lists of data record tuples.

---

### 84. How do you find the nth highest value in a column of a table?
You find it using subqueries with `LIMIT` / `OFFSET` or using the `DENSE_RANK()` window function.

```sql
-- Retrieve 3rd highest salary value using DENSE_RANK window function
WITH RankedSalaries AS (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM company_employees
)
SELECT DISTINCT salary 
FROM RankedSalaries 
WHERE rnk = 3;
```

**Code Logic:**  
1. The CTE calculates dense sequential salary ranks without value gaps ordered highest-to-lowest.  
2. The outer query filters for `rnk = 3` to output the exact 3rd highest salary value.

**Terms Used:**
  * `DENSE_RANK`: Analytic ranking function handling tie values without rank sequence gaps.
  * `OVER`: Clause establishing ordered calculation windows.

---

### 85. How do you find the values in a text column of a table that start with a certain letter?
You find them using the `LIKE` pattern matching operator combined with the `%` wildcard character.

```sql
-- Retrieve customers whose names begin with uppercase letter 'A'
SELECT customer_id, customer_name 
FROM customer_directory 
WHERE customer_name LIKE 'A%';
```

**Code Logic:**  
The string pattern `'A%'` matches text entries in `customer_name` that begin strictly with the letter `'A'`, while allowing `%` to match any trailing character sequence of any length.

**Terms Used:**
  * `LIKE`: Pattern matching evaluation operator.
  * `%`: Wildcard character matching any dynamic sequence of trailing characters.

---

### 86. How do you find the last id in a table?
You find it using `MAX(id)` or sorting `ORDER BY id DESC` with a limit of 1.

```sql
-- Calculate peak primary key ID allocated in sales table
SELECT MAX(order_id) AS last_order_id 
FROM sales_orders;
```

**Code Logic:**  
The aggregate function `MAX(order_id)` scans the primary index key values in `sales_orders` and instantly extracts the largest numerical identifier currently assigned in the table.

**Terms Used:**
  * `MAX`: Aggregate function evaluating the highest numerical or scalar value in a column.

---

### 87. How to select random rows from a table?
You select them by ordering rows by random generator functions like `RAND()`, `RANDOM()`, or `NEWID()` and taking a limit.

```sql
-- PostgreSQL syntax returning 3 random sample items
SELECT product_id, product_name 
FROM product_catalog 
ORDER BY RANDOM() 
LIMIT 3;
```

**Code Logic:**  
The statement assigns a pseudo-random floating numeric value to each row using `RANDOM()`. `ORDER BY RANDOM()` sorts the dataset dynamically based on these values, and `LIMIT 3` captures the top three rows.

**Terms Used:**
  * `RANDOM`: Pseudo-random generation function yielding dynamic sort keys.
  * `LIMIT`: Clause capping maximum returned row output counts.

---

## Section 5: Scenario-Based SQL Questions

### 88. How do you find and remove duplicate records from a table?
Find duplicates using `GROUP BY` and `HAVING COUNT(*) > 1`. Remove duplicates by isolating distinct IDs using CTEs paired with window functions like `ROW_NUMBER()`.

```sql
-- Isolate and delete duplicate email records retaining lowest ID
WITH UserDuplicates AS (
    SELECT user_id, email,
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY user_id) AS row_num
    FROM user_accounts
)
DELETE FROM user_accounts 
WHERE user_id IN (SELECT user_id FROM UserDuplicates WHERE row_num > 1);
```

**Code Logic:**  
The CTE partitions rows by `email` and numbers duplicate instances using `ROW_NUMBER()`. Unique primary entries receive `row_num = 1`. The `DELETE` statement targets all `user_id` records where `row_num > 1`, purging duplicates.

**Terms Used:**
  * `ROW_NUMBER`: Analytic window function assigning incremental row counts per partition.
  * `PARTITION BY`: Clause grouping record partitions for window evaluations.
  * `DELETE`: Command removing isolated duplicate rows from tables.

---

### 89. How do you calculate a running total (cumulative sum)?
You calculate it using the `SUM()` aggregate function as a window function paired with `OVER (ORDER BY ...)`.

```sql
-- Calculate cumulative running sales total over date sequences
SELECT transaction_date, amount,
       SUM(amount) OVER (ORDER BY transaction_date) AS running_total
FROM daily_sales;
```

**Code Logic:**  
The window function `SUM(amount) OVER (ORDER BY transaction_date)` aggregates daily `amount` values cumulatively, adding the current row's sales amount to the cumulative sum of all prior ordered dates.

**Terms Used:**
  * `SUM`: Aggregate calculation function computing total numeric sums.
  * `OVER`: Clause defining cumulative window calculation frames ordered by date.

---

### 90. How do you find employees who earn more than the average salary in their department?
Use a correlated subquery comparing individual employee salary against department average, or a CTE calculating department averages.

```sql
-- Compare employee salary against localized department average subquery
SELECT e.emp_name, e.dept_id, e.salary
FROM staff_members e
WHERE e.salary > (
    SELECT AVG(sub.salary) 
    FROM staff_members sub 
    WHERE sub.dept_id = e.dept_id
);
```

**Code Logic:**  
For each employee row `e`, the correlated subquery executes `SELECT AVG(...)` filtered specifically by matching `sub.dept_id = e.dept_id`. The outer query retains only employees whose `salary` exceeds their department's average.

**Terms Used:**
  * `Correlated Subquery`: Inner query evaluating row-by-row using outer query values.
  * `AVG`: Aggregate calculation function computing average numerical salaries.

---

### 91. How do you find gaps in a sequence of numbers (e.g., missing invoice numbers)?
Use the `LEAD()` window function to evaluate the next sequential value and check if `next_value - current_value > 1`.

```sql
-- Detect missing sequence bounds across numbered invoice logs
WITH SequenceChecker AS (
    SELECT invoice_id AS current_id,
           LEAD(invoice_id) OVER (ORDER BY invoice_id) AS next_id
    FROM billing_invoices
)
SELECT current_id + 1 AS gap_start, next_id - 1 AS gap_end
FROM SequenceChecker
WHERE next_id - current_id > 1;
```

**Code Logic:**  
`LEAD(invoice_id)` inspects the next ordered row's ID value. If `next_id - current_id > 1`, a sequence gap exists. The outer query calculates `current_id + 1` and `next_id - 1` to define start and end bounds of missing IDs.

**Terms Used:**
  * `LEAD`: Analytic window function fetching column values from subsequent rows.
  * `OVER`: Clause establishing ordered analysis windows across sequence records.

---

### 92. How do you find customers who made purchases in consecutive months?
Extract purchase months using date truncation/formatting, use `LAG()` to pull previous purchase month, and check for 1-month differences.

```sql
-- Identify customers placing orders across back-to-back consecutive months
WITH MonthlyPurchases AS (
    SELECT DISTINCT customer_id, 
           DATE_TRUNC('month', order_date) AS purchase_month
    FROM customer_orders
),
SequentialAnalysis AS (
    SELECT customer_id, purchase_month,
           LAG(purchase_month) OVER (PARTITION BY customer_id ORDER BY purchase_month) AS prev_month
    FROM MonthlyPurchases
)
SELECT DISTINCT customer_id 
FROM SequentialAnalysis 
WHERE purchase_month = prev_month + INTERVAL '1 month';
```

**Code Logic:**  
`DATE_TRUNC()` normalizes order dates into monthly blocks. `LAG()` retrieves each customer's previous active purchase month. The outer query filters for rows where `purchase_month` equals `prev_month + 1 month`.

**Terms Used:**
  * `LAG`: Analytic window function pulling field data from preceding rows.
  * `DATE_TRUNC`: Date function truncating timestamps to specified precision levels (e.g., month).

---

### 93. How do you pivot data from rows to columns?
Transform row values into column headers using conditional aggregation (`SUM(CASE ...)` or `FILTER`) or native `PIVOT` statements.

```sql
-- Pivot quarterly sales rows into horizontal summary display columns
SELECT fiscal_year,
       SUM(CASE WHEN quarter = 'Q1' THEN sales_amount ELSE 0 END) AS Q1_Sales,
       SUM(CASE WHEN quarter = 'Q2' THEN sales_amount ELSE 0 END) AS Q2_Sales
FROM quarterly_report_data
GROUP BY fiscal_year;
```

**Code Logic:**  
The query groups records by `fiscal_year`. Inside the aggregate `SUM()` functions, `CASE` statements evaluate row quarters, routing sales amounts into explicit output columns (`Q1_Sales`, `Q2_Sales`) based on matching values.

**Terms Used:**
  * `SUM`: Aggregate function accumulating calculated sales metric values.
  * `CASE`: Conditional expression routing quarter amounts to target pivot headers.

---

### 94. How do you find the top 3 products by sales in each category?
Rank products within each category using `DENSE_RANK()` or `ROW_NUMBER()` in a CTE, then filter for `rank <= 3`.

```sql
-- Rank products by sales within category partitions and output top 3
WITH PartitionedProducts AS (
    SELECT category_id, product_name, total_sales,
           DENSE_RANK() OVER (PARTITION BY category_id ORDER BY total_sales DESC) AS rnk
    FROM catalog_products
)
SELECT category_id, product_name, total_sales 
FROM PartitionedProducts 
WHERE rnk <= 3;
```

**Code Logic:**  
The CTE uses `DENSE_RANK()` partitioned by `category_id` and ordered by `total_sales DESC` to assign ranks per category. The outer query filters `WHERE rnk <= 3` to select only top 3 products for each category group.

**Terms Used:**
  * `DENSE_RANK`: Ranking window function handling tied sales totals without rank sequence gaps.
  * `PARTITION BY`: Clause partitioning calculation windows by individual category groups.

---

### 95. What are ACID properties in database transactions?
ACID ensures database transactional reliability:
* **Atomicity:** All-or-nothing completion.
* **Consistency:** Keeps database state valid according to constraints.
* **Isolation:** Concurrent transactions don't interfere with each other.
* **Durability:** Committed changes persist even after system crashes.

```sql
-- Transactional block maintaining ACID properties across two updates
BEGIN TRANSACTION;
    UPDATE user_accounts SET balance = balance - 100.00 WHERE account_id = 10;
    UPDATE user_accounts SET balance = balance + 100.00 WHERE account_id = 20;
COMMIT;
```

**Code Logic:**  
The transaction block ensures both `UPDATE` statements execute as a single atomic unit of work. If both succeed, `COMMIT` writes changes permanently; if an error occurs, the engine rolls back all updates to keep data consistent.

**Terms Used:**
  * `BEGIN TRANSACTION`: Command marking the start boundary of an atomic transaction.
  * `COMMIT`: Command permanently saving transactional database modifications.

---

### 96. What is a deadlock, and how do you prevent it?
A deadlock occurs when two or more concurrent transactions block each other indefinitely by holding locks on resources the other needs. Prevent by acquiring locks in a consistent order and keeping transactions short.

```sql
-- Prevent deadlocks by enforcing consistent table lock access order
BEGIN;
    UPDATE customer_accounts SET balance = balance - 50.00 WHERE account_id = 101;
    UPDATE customer_accounts SET balance = balance + 50.00 WHERE account_id = 202;
COMMIT;
```

**Code Logic:**  
Deadlocks are avoided by ensuring all application transactions update accounts in strict ascending ID sequence (updating account `101` before `202`). This predictable locking order eliminates cyclic lock waiting dependencies.

**Terms Used:**
  * `BEGIN`: Statement marking transaction execution start bounds.
  * `UPDATE`: Data manipulation command acquiring row update locks.

---

### 97. How do you optimize a slow-running SQL query?
Add indexes on search/join columns, avoid `SELECT *`, eliminate unindexed wildcards (like `%term`), examine execution plans (`EXPLAIN`), and replace correlated subqueries with JOINs/CTEs.

```sql
-- Analyze physical execution strategy to identify index optimization needs
EXPLAIN ANALYZE 
SELECT id, username, email 
FROM app_users 
WHERE status_code = 'ACTIVE' AND department_id = 12;
```

**Code Logic:**  
By requesting explicitly needed columns (`id`, `username`, `email`) rather than `SELECT *`, and evaluating runtime execution metrics using `EXPLAIN ANALYZE`, developers locate cost bottlenecks and missing index requirements.

**Terms Used:**
  * `EXPLAIN ANALYZE`: Profiling diagnostic command displaying query planner execution execution plans.
  * `SELECT`: Query projection clause explicitly listing desired output fields.

---

### 98. How do you handle NULL values in calculations and comparisons?
Handle `NULL` explicitly using functions like `COALESCE()`, `IFNULL()`, or `NVL()`, and evaluate condition states with `IS NULL` / `IS NOT NULL`.

```sql
-- Replace NULL values with zero fallback during compensation arithmetic
SELECT emp_name, salary + COALESCE(bonus_amount, 0.00) AS total_compensation 
FROM company_staff;
```

**Code Logic:**  
`COALESCE(bonus_amount, 0.00)` inspects `bonus_amount`. If the field contains a valid number, it outputs that number; if the field is `NULL`, it outputs `0.00`, preventing addition operations from resolving to `NULL`.

**Terms Used:**
  * `COALESCE`: Scalar function returning the first non-null argument in its parameter list.

---

### 99. How do you find the longest streak of consecutive login days for a user?
Form group identifiers by subtracting dense rank values from event dates (`login_date - ROW_NUMBER()`), group by that resulting identifier, count days per block, and find the maximum block length.

```sql
-- Identify maximum consecutive login streak per user using island grouping
WITH DistinctLogins AS (
    SELECT DISTINCT user_id, CAST(login_time AS DATE) AS login_date
    FROM user_login_logs
),
IslandGroups AS (
    SELECT user_id, login_date,
           login_date - (ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) * INTERVAL '1 day') AS streak_group
    FROM DistinctLogins
),
StreakCalculations AS (
    SELECT user_id, COUNT(*) AS streak_length
    FROM IslandGroups
    GROUP BY user_id, streak_group
)
SELECT user_id, MAX(streak_length) AS longest_streak
FROM StreakCalculations
GROUP BY user_id;
```

**Code Logic:**  
1. `DistinctLogins` deduplicates login dates.  
2. `IslandGroups` subtracts row numbers from `login_date` to generate identical `streak_group` date markers for unbroken consecutive login runs.  
3. `StreakCalculations` groups by `streak_group` to count run lengths.  
4. The main query evaluates `MAX(streak_length)` per user.

**Terms Used:**
  * `ROW_NUMBER`: Analytic window function used to generate sequential sequence markers.
  * `PARTITION BY`: Clause isolating streak calculations per individual user.
  * `MAX`: Aggregate function returning the largest calculate streak length integer.

---

