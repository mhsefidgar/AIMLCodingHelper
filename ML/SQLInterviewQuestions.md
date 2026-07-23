
# SQL Questions and Answers by Mohamamd Sefidgar

## Section 1: General SQL Questions

### 1. What is SQL?
SQL (Structured Query Language) is the standard domain-specific language designed for managing, querying, and manipulating data stored in relational database management systems (RDBMS). We use SQL because it provides a standardized, declarative, and efficient way to store, retrieve, filter, and alter structured data across various database engines without needing to write low-level file manipulation logic.

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

**Best Practices:**
  * Avoid using `SELECT *` in production code; explicitly name only the required columns to reduce unnecessary I/O, network traffic, and memory consumption.
  * Ensure filter columns used in `WHERE` clauses (like `dept_id` and `status`) are properly indexed to speed up query execution.

---

### 2. What are SQL dialects? Give some examples.
SQL dialects are specialized implementations or extended variations of the ANSI/ISO standard SQL developed by different database vendors (such as PostgreSQL, MySQL, Oracle, and Microsoft SQL Server). We have dialects because database engines implement distinct underlying architectures, storage engines, and specialized features; dialects allow vendors to add custom optimization capabilities, specialized functions, and performance enhancements tailored to their specific database systems.

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

**Best Practices:**
  * Stick to standard ANSI SQL syntax whenever possible to ensure maximum code portability across different database platforms.
  * Abstract vendor-specific dialect queries into specialized database access layers or view abstractions when application migration flexibility is required.

---

### 3. What are the main applications of SQL?
The main applications of SQL involve Data Definition (DDL), Data Manipulation (DML), Data Querying (DQL), Data Control (DCL), and Transaction Control (TCL) across applications like data warehousing, business intelligence, application backends, and reporting systems. We use SQL for these applications because relational databases require a secure, reliable, transaction-safe, and ACID-compliant method to store state, enforce business logic, execute reporting metrics, and manage user permissions seamlessly.

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

**Best Practices:**
  * Always explicitly list the target column names in `INSERT` statements to avoid unexpected failures if table schemas change in the future.
  * Never run an `UPDATE` command without a precise `WHERE` clause unless you intentionally intend to update every single row in the target table.

---

## Section 2: SQL Questions for Beginners

### 4. What is an SQL statement?
An SQL statement is a complete, syntactically correct command or instruction sent to a relational database engine to execute a specific action—such as creating a table, modifying permissions, or querying records. We use SQL statements to formally communicate intended administrative tasks, schema changes, or transactional operations to the database system in a controlled, declarative syntax.

```sql
-- Remove a temporary logs table safely if it exists in the schema
DROP TABLE IF EXISTS audit_logs_temp;
```

**Code Logic:**  
This DDL statement instructs the database server to check if a table named `audit_logs_temp` exists in the active schema. If present, the engine deletes the table structure and its physical data storage pages permanently.

**Terms Used:**
  * `SQL Statement`: A complete, executable command sent to a database engine to perform an action.
  * `DROP TABLE`: A Data Definition Language (DDL) command that permanently destroys an existing table.

**Best Practices:**
  * End every SQL statement with a semicolon `;` to adhere to ANSI standards and avoid syntax errors when batching multiple execution statements.
  * Use safety checks like `IF EXISTS` when running destructive DDL statements in automated scripts to avoid execution pipeline errors.

---

### 5. What is an SQL query?
An SQL query is a specific subset of SQL statements focused primarily on requesting, retrieving, filtering, and projecting data out of database tables without necessarily altering the underlying persistent storage. We use SQL queries to answer business questions, drive application UI outputs, perform analytics, and transform data into actionable information in real time.

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

**Best Practices:**
  * Filter early in the query process using precise conditional expressions inside the `WHERE` clause to avoid processing unnecessary records.
  * Avoid placing computational functions directly on indexed columns in `WHERE` clauses, as this can break index usage (sargability).

---

### 6. What is an SQL subquery?
An SQL subquery is a nested query block embedded inside another parent SQL statement (such as `SELECT`, `INSERT`, `UPDATE`, or `DELETE`) to compute intermediate values or result sets used by the outer query. We use subqueries to break complex, multi-step logical operations into single declarative expressions, such as comparing individual records against calculated group summary aggregates.

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

**Best Practices:**
  * Ensure subqueries returning scalar values for single comparisons are guaranteed to yield only one row and column to avoid runtime database exceptions.
  * For complex multi-row checks, consider evaluating performance differences between subqueries, `JOIN` operations, or Common Table Expressions (CTEs).

---

### 7. What is an SQL join?
An SQL join is a relational operation that merges records from two or more database tables into unified output rows based on a specified relationship between shared key columns. We use joins to normalize data across separate, focused entity tables while maintaining the ability to combine and query related information holistically without data duplication.

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

**Best Practices:**
  * Always explicitly qualify joined column names with table names or aliases (e.g., `orders.customer_id`) to avoid ambiguous column lookup errors.
  * Verify that foreign keys and join condition target columns are indexed on both joining tables to maintain optimal execution speeds.

---

### 8. What is an SQL comment?
An SQL comment is developer documentation or metadata text embedded directly within SQL code files that is deliberately ignored by the query parser and execution engine during runtime. We use comments to explain complex query logic, document assumptions, track authoring notes, or temporarily disable code blocks during testing and debugging without destroying written code.

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

**Best Practices:**
  * Write clean, self-documenting SQL first; reserve comments for explaining *why* complex choices or business edge-cases exist rather than stating obvious operations.
  * Remove commented-out dead code blocks prior to deploying database scripts or queries into production environments.

---

### 9. What is an SQL alias?
An SQL alias is a temporary, query-scoped alternate name assigned to a table or column using the `AS` keyword. We use aliases to shorten long or fully-qualified table names (improving query readability), clear up naming ambiguities during joins, and provide readable, descriptive display labels for computed or aggregated expression columns in final result sets.

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

**Best Practices:**
  * Use meaningful, intuitive short aliases (e.g., `emp` for `employees`) rather than non-descriptive single letters when handling multi-table complex queries.
  * Always alias computed columns or function outputs so downstream applications receive consistent, explicitly named response keys.

---

## Section 3: Technical SQL Questions

### 10. What types of SQL commands do you know?
SQL commands are categorized based on their functionality into five core subsets: Data Definition Language (DDL), Data Manipulation Language (DML), Data Query Language (DQL), Data Control Language (DCL), and Transaction Control Language (TCL). We use these distinct command classifications to logically segregate database engine operations—separating schema architecture management (DDL) and security access control (DCL) from daily data processing (DML/DQL) and transaction safety (TCL).

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

**Best Practices:**
  * Separate DDL deployment scripts from operational DML application queries to prevent accidental schema modifications during routine executions.
  * Group DML operations inside TCL transaction boundaries (`BEGIN`, `COMMIT`, `ROLLBACK`) when executing related multi-table data updates.

---

### 11. Give some examples of common SQL commands.
Common SQL commands represent foundational operations across SQL categories, such as `CREATE`, `ALTER`, and `DROP` (DDL); `INSERT`, `UPDATE`, and `DELETE` (DML); `SELECT` (DQL); `GRANT` and `REVOKE` (DCL); and `COMMIT` and `ROLLBACK` (TCL). We use these standard commands to perform essential database lifecycle tasks—from building structures and populating tables to securing access permissions and executing data retrieval.

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

**Best Practices:**
  * Consistently format standard SQL keywords in UPPERCASE to maintain readable, well-structured script conventions.
  * Verify script execution rights and test destructive commands (`DROP`, `DELETE`) in staging environments prior to production usage.

---

### 12. What is DBMS, and what types of DBMS do you know?
A Database Management System (DBMS) is software that serves as the administrative interface between databases, end users, and applications to capture, manage, store, and analyze data. Major types include Relational DBMS (RDBMS), NoSQL Document Stores, Key-Value Stores, Column-Family Stores, Graph DBMS, Object-Oriented DBMS, and Hierarchical DBMS. We use DBMS platforms to provide multi-user access control, guarantee data persistent storage, maintain data integrity, enforce backup safety, and abstract complex hardware disk operations.

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

**Best Practices:**
  * Select the specific DBMS type based on your application's data structure, scale needs, transaction integrity requirements, and workload patterns.
  * Configure DBMS security settings, connection pooling, and memory allocations according to production workload expectations.

---

### 13. What is RDBMS? Give some examples of RDBMS.
A Relational Database Management System (RDBMS) is a specialized class of DBMS that organizes data into structured, two-dimensional tables consisting of rows and columns linked together by predefined relationships (foreign keys). Popular examples include PostgreSQL, MySQL, Microsoft SQL Server, Oracle Database, and SQLite. We use RDBMS systems because they strictly support ACID properties, enforce strict schema boundaries, eliminate redundancy, and ensure high data consistency across enterprise datasets.

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

**Best Practices:**
  * Design well-structured primary and foreign key constraints on RDBMS schemas to leverage internal query optimizations and maintain referential integrity.
  * Standardize software versions across development and production RDBMS instances to prevent dialect behavior mismatches.

---

### 14. What are tables and fields in SQL?
In relational databases, a table (or relation) is a logical collection of related data organized into horizontal rows (records) and vertical columns (fields or attributes). A field represents an individual, named attribute within a table that holds a distinct data value of a specific data type (such as an integer, string, or date). We use tables and fields to partition data into structured entities, allowing applications to store, retrieve, and filter explicit attributes cleanly.

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

**Best Practices:**
  * Assign clear, singular entity names to tables (e.g., `user_account`) or consistent plural names (e.g., `user_accounts`) across your schema design.
  * Choose the most compact, precise data type available for each field to minimize storage footprint and optimize index lookup performance.

---

### 15. What types of SQL subqueries do you know?
SQL subqueries are categorized based on their execution behavior and returned structure into Single-Row Subqueries (returns a single value), Multi-Row Subqueries (returns multiple row values), Multi-Column Subqueries, Nested Subqueries, and Correlated Subqueries (which depend on the outer query's row value). We use these distinct subquery types depending on the logical scope required—ranging from filtering against single scalar thresholds to performing dynamic evaluations across sets.

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

**Best Practices:**
  * Avoid heavy correlated subqueries on large datasets; rewrite them as `JOIN` operations or window functions when processing speed is critical.
  * Ensure subqueries evaluated with scalar comparison operators (`=`, `>`, `<`) strictly handle `NULL` cases and return exactly one row.

---

### 16. What is a constraint, and why use constraints?
A constraint is a programmatic rule or restriction enforced by the database engine on table columns to restrict the values that can be inserted, updated, or stored. We use constraints to protect data integrity, prevent invalid or corrupt data from entering physical tables, enforce business logic rules at the database level, and guarantee accurate relationships between entities regardless of application code bugs.

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

**Best Practices:**
  * Enforce constraints at the database tier rather than relying purely on client-side application validation to protect data safety.
  * Assign explicit, descriptive constraint names (e.g., `chk_bank_accounts_balance_positive`) to make debugging runtime constraint violations easier.

---

### 17. What SQL constraints do you know?
Common SQL constraints include `NOT NULL` (prevents null values), `UNIQUE` (ensures all column values are distinct), `PRIMARY KEY` (uniquely identifies rows), `FOREIGN KEY` (enforces referential integrity), `CHECK` (verifies specific conditions), and `DEFAULT` (provides fallback values). We use these targeted constraints to establish schema validity rules, enforce record identity, build relational ties, and ensure overall database quality.

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

**Best Practices:**
  * Always mark required business attributes with `NOT NULL` constraints to prevent unintended missing data issues.
  * Use `DEFAULT` values for status, boolean, and timestamp tracking fields to simplify insert logic across application code bases.

---

### 18. What types of joins do you know?
SQL join operations include `INNER JOIN` (returns matching records in both tables), `LEFT (OUTER) JOIN` (returns all left table records and matched right records), `RIGHT (OUTER) JOIN` (returns all right table records), `FULL (OUTER) JOIN` (returns all records when a match exists in either table), `CROSS JOIN` (produces Cartesian product), and `SELF JOIN` (joins a table to itself). We use these variations to extract precise subsets of interconnected data based on whether non-matching outer records must be preserved or excluded.

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

**Best Practices:**
  * Prefer `LEFT JOIN` over `RIGHT JOIN` for query readability, structuring queries logically from left to right.
  * Avoid unintentional `CROSS JOIN` operations caused by missing or incomplete `ON` clauses, as they can cause severe engine performance degradation.

---

### 19. What is a primary key in SQL?
A primary key is a column (or combination of columns) that uniquely identifies every distinct row within a database table. A primary key strictly enforces uniqueness, automatically creates a unique index, and forbids `NULL` values. We use primary keys to give every record a definitive, immutable entity identity required for reliable row updates, precise lookups, and referential integrity via foreign key links.

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

**Best Practices:**
  * Keep primary keys short, numerical, and unchanging (e.g., auto-incrementing integers or UUIDs) rather than using volatile business text fields.
  * Ensure every operational database table has a designated primary key to preserve entity identity and facilitate replication.

---

### 20. What is a unique key in SQL?
A unique key is a constraint that guarantees all stored values across a target column or group of columns remain distinct from one another. Unlike a primary key, a table can contain multiple unique key constraints, and depending on the database engine, unique keys allow the storage of single or multiple `NULL` values. We use unique keys to enforce domain uniqueness on secondary business fields (such as email addresses, government IDs, or handles) without making them the primary clustered key.

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

**Best Practices:**
  * Use unique key constraints for non-primary key attributes that require business uniqueness (like usernames or tax IDs).
  * Be aware of how your specific database engine handles `NULL` values in `UNIQUE` constraints (e.g., PostgreSQL allows multiple `NULL`s, whereas older engines restrict them).

---

### 21. What is a foreign key in SQL?
A foreign key is a column or set of columns in a child table that references a primary key or unique key in a parent table. We use foreign keys to build formal relational connections between tables and enforce referential integrity—preventing orphaned records by ensuring that child table entries cannot point to non-existent parent records, and preventing parent records from being deleted while dependent child records exist.

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

**Best Practices:**
  * Always add database indexes to foreign key columns to drastically speed up join performance and cascade operations.
  * Explicitly define foreign key cascade behavior (e.g., `ON DELETE RESTRICT` or `ON DELETE CASCADE`) based on exact application domain logic.

---

### 22. What is an SQL index?
An SQL index is a specialized physical data structure (typically implemented as a B-Tree or Hash index) created on specific table columns to rapidly locate and access target data rows without scanning every row in a table. We use indexes to dramatically improve query search performance, accelerate `WHERE` filtering, speed up `JOIN` condition matching, and optimize sorting operations at the trade-off of extra disk space and slightly slower write operations (`INSERT`, `UPDATE`, `DELETE`).

```sql
-- Build an index on customer email addresses to accelerate search filters
CREATE INDEX idx_customers_email ON app_customers(email_address);
```

**Code Logic:**  
The engine constructs a background B-tree lookup structure (`idx_customers_email`) specifically referencing `email_address`. Subsequent queries using `WHERE email_address = 'user@test.com'` perform fast index seeks instead of scanning the full table sequentially.

**Terms Used:**
  * `CREATE INDEX`: A DDL command building a fast lookup structure for designated table columns.
  * `Index`: A database search structure created to speed up data query operations.

**Best Practices:**
  * Index columns that are frequently used in `WHERE` clauses, `JOIN` conditions, and `ORDER BY` specifications.
  * Avoid over-indexing tables with extremely high write/insert volumes, as every index must be re-balanced during data modifications.

---

### 23. What types of indexes do you know?
SQL supports multiple specialized index structures including Clustered Indexes (determines physical row storage order), Non-Clustered Indexes (separate pointer lookup structure), Unique Indexes, Composite Indexes (built on multiple combined columns), Partial/Filtered Indexes, and Full-Text Indexes. We use these distinct index types to optimize tailored workload patterns—such as multi-column searches, text search engines, unique field enforcement, or spatial queries.

```sql
-- Create a composite index spanning two lookup columns simultaneously
CREATE INDEX idx_emp_name_dept ON employees(last_name, department_id);
```

**Code Logic:**  
The system creates a composite index containing ordered keys for both `last_name` and `department_id`. Queries filtering on both columns simultaneously utilize this multi-column structure for optimized data retrieval.

**Terms Used:**
  * `Composite Index`: An index constructed using two or more combined table columns.
  * `CREATE INDEX`: Command creating secondary index structures on existing tables.

**Best Practices:**
  * Order columns in composite indexes according to selectivity—placing the most restrictive or frequently filtered column first (left-prefix rule).
  * Monitor index usage metrics regularly and drop unused or redundant indexes to recover write performance and memory.

---

### 24. What is a schema?
A database schema is a logical container and structural blueprint within a database that organizes related database objects—including tables, views, indexes, stored procedures, constraints, and data types—under a unified namespace. We use schemas to partition multi-tenant applications, group related domain entities logically, manage granular database security access permissions, and prevent naming collisions across system modules.

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

**Best Practices:**
  * Use schemas to enforce security boundaries, assigning restrictive role permissions per schema (e.g., read-only analytics access to an reporting schema).
  * Fully qualify object names (e.g., `schema_name.table_name`) in application code to prevent environment search-path ambiguity.

---

### 25. What is an SQL operator?
An SQL operator is a reserved symbol, character sequence, or keyword used in SQL statements to perform mathematical, relational, comparison, or logical evaluations on values and expressions. We use operators to alter values, build dynamic search expressions in `WHERE` clauses, calculate numeric quantities, and manage multi-condition logical operations within queries.

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

**Best Practices:**
  * Use parentheses `()` explicitly when combining multiple logical operators (`AND`, `OR`) to clarify evaluation precedence.
  * Be cautious with comparison operators when handling columns that contain `NULL` values, as standard comparisons against `NULL` yield unknown.

---

### 26. What types of SQL operators do you know?
SQL operators are categorized into Arithmetic (`+`, `-`, `*`, `/`, `%`), Comparison (`=`, `!=`, `>`, `<`, `>=`, `<=`), Logical (`AND`, `OR`, `NOT`), Bitwise, and Special Set/Range operators (`IN`, `BETWEEN`, `LIKE`, `IS NULL`, `EXISTS`). We use these distinct operator categories to conduct exact calculations, apply conditional filtering, compare text patterns, inspect null states, and evaluate relational set memberships within query conditions.

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

**Best Practices:**
  * Remember that `BETWEEN` is inclusive of both endpoint values; use explicit comparison boundaries (`>=` and `<`) if upper limits must be excluded.
  * Prefer `IN` or `EXISTS` over long chains of `OR` statements for clearer code and better query optimizer plan choices.

---

### 27. What is a clause?
A clause is a built-in, mandatory or optional structural component of an SQL statement used to define specific operational logic—such as filtering rows (`WHERE`), specifying data sources (`FROM`), grouping sets (`GROUP BY`), filtering groups (`HAVING`), or ordering results (`ORDER BY`). We use clauses as structural blocks to compose declarative data requests, guiding the database engine on how to extract, process, aggregate, and present required data.

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

**Best Practices:**
  * Structure clauses in consistent, standard written order to ensure valid syntactic compilation and make queries readable.
  * Keep clauses concise; push filtering logic into `WHERE` clauses rather than doing redundant processing in `HAVING` or projection stages.

---

### 28. What are some common statements used with the SELECT query?
Common clauses and statement keywords used alongside `SELECT` queries include `FROM`, `WHERE`, `JOIN`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT` / `TOP`, and `DISTINCT`. We use these companion statement components together to construct precise query processing pipelines—filtering source records, joining entities, generating summary aggregations, sorting rows, and bounding final output volumes.

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

**Best Practices:**
  * Follow correct clause order (`SELECT` -> `FROM` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `ORDER BY`) to prevent compilation errors.
  * Use `HAVING` exclusively for filtering aggregate properties; put non-aggregate scalar condition filters in the `WHERE` clause.

---

### 29. How do you create a table in SQL?
You create a table in SQL using the `CREATE TABLE` Data Definition Language (DDL) statement, followed by specifying the table name, column field definitions, appropriate data types, and required schema constraints (such as `PRIMARY KEY` or `NOT NULL`). We use `CREATE TABLE` statements to establish new physical structures within a database to persist organized entity data systematically.

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

**Best Practices:**
  * Always explicitly specify data types and length limits for string fields (`VARCHAR(N)`) to enforce proper data boundary allocations.
  * Include standard audit columns (e.g., `created_at`, `updated_at`) during table creation to maintain historical tracking data.

---

### 30. How to update a table?
You update existing table data using the `UPDATE` Data Manipulation Language (DML) statement, specifying the target table, the `SET` clause with modified column values, and a `WHERE` clause to bound target records. We use update operations to alter existing stored attribute values across database rows when underlying business data changes over time.

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

**Best Practices:**
  * Always test the `WHERE` condition using a `SELECT` query first to verify exactly which rows will be affected before issuing an `UPDATE`.
  * Wrap bulk or critical updates within an explicit transaction block (`BEGIN` / `COMMIT`) so changes can be rolled back if an error occurs.

---

### 31. How to delete a table from a database?
You delete an entire table permanently using the `DROP TABLE` Data Definition Language (DDL) command. We use `DROP TABLE` when a data structure and its associated stored records, indexes, triggers, and permissions are no longer needed and must be completely removed from the physical database system and system catalog.

```sql
-- Remove session table completely from current schema
DROP TABLE temporary_user_sessions;
```

**Code Logic:**  
The engine executes a DDL operation that instantly drops the object `temporary_user_sessions` from the catalog, releasing stored table rows, associated index structures, and structural schema definitions permanently.

**Terms Used:**
  * `DROP TABLE`: A DDL statement permanently destroying tables and their records.

**Best Practices:**
  * Use `DROP TABLE IF EXISTS` in automated deployment or migration scripts to prevent script failures if the object does not exist.
  * Maintain database backups and double-check object names before dropping tables, as this action cannot be undone with standard rollbacks outside open transactions.

---

### 32. How to get the count of records in a table?
You calculate the total number of records in a table using the `COUNT()` aggregate function within a `SELECT` statement, passing either `*` or specific column names. We use `COUNT()` to gather dataset size metrics, perform statistical reporting, check data availability, and evaluate group sizes during analytical queries.

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

**Best Practices:**
  * Use `COUNT(*)` when you want to count total physical rows regardless of `NULL` values across columns.
  * Use `COUNT(column_name)` specifically when you want to ignore `NULL` entries present in that designated column.

---

### 33. How to sort records in a table?
You sort output records using the `ORDER BY` clause placed near the end of a `SELECT` query, specifying target sorting columns and ordering direction (`ASC` for ascending or `DESC` for descending). We use sorting to present data in meaningful sequence orders—such as chronological dates, alphabetical names, or sorted financial values for reports and client applications.

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

**Best Practices:**
  * Ensure sorting on large tables is backed by an index on the `ORDER BY` target column to prevent expensive external disk-sorting operations.
  * Avoid relying on implicit database physical insertion order; always explicitly include an `ORDER BY` clause if consistent output ordering is required.

---

### 34. How to select all columns from a table?
You select every defined column in a table using the asterisk wildcard symbol (`*`) in the projection list of a `SELECT` query. We use `SELECT *` primarily during ad-hoc exploratory analysis and debugging to quickly inspect table structures and complete record samples.

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

**Best Practices:**
  * Limit `SELECT *` to interactive debugging session testing; avoid using it in application production code to prevent unexpected memory overhead and breaking schema changes.
  * explicitly list required column names in production queries to optimize database I/O performance and application object mapping safety.

---

### 35. How to select common records from two tables?
You extract matching or common records between two tables by performing an `INNER JOIN` on matching relational key columns or by executing the `INTERSECT` set operator across two structural queries. We use these methods to isolate intersecting data entities—such as identifying users who exist simultaneously in both active staff directories and specialized system admin logs.

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

**Best Practices:**
  * Choose `INNER JOIN` when you need to project combined columns from both tables simultaneously.
  * Choose `INTERSECT` when you want to isolate unique common set keys across two uniform dataset queries cleanly.

---

### 36. What is the DISTINCT statement, and how do you use it?
The `DISTINCT` keyword is a query modifier placed immediately after `SELECT` to eliminate duplicate rows from a query's result set, returning only unique values across target projection columns. We use `DISTINCT` when querying tables that contain repeating values to extract clean, deduplicated categorical lists or metric options.

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

**Best Practices:**
  * Use `DISTINCT` purposefully; applying it unnecessarily can introduce performance overhead because the database engine must perform sorting or hashing to deduplicate rows.
  * Consider using `GROUP BY` instead if you plan to combine deduplication with aggregate metric calculations.

---

### 37. What are relationships? Give some examples.
Relationships are formal logical associations established between different database tables using Primary Key and Foreign Key constraints. Standard relationship types include One-to-One (1:1), One-to-Many (1:N), and Many-to-Many (M:N). We use relationships to structure normalized data entities separately while preserving the relational framework required to link, model, and query real-world business domains accurately.

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

**Best Practices:**
  * Map Many-to-Many (M:N) relationships using a explicit junction/bridge table containing composite foreign keys.
  * Maintain referential constraints across relationships to prevent corrupted or orphaned record references.

---

### 38. What is a NULL value? How is it different from zero or a blank space?
A `NULL` value in SQL is a special state marker indicating that a data value is missing, unknown, unassigned, or non-existent. It is fundamentally different from a numeric zero (`0`) or an empty string (`''`), which are defined literal values. We use `NULL` to represent unknown or unpopulated attributes in a database without fabricating inaccurate placeholder data.

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

**Best Practices:**
  * Always use `IS NULL` or `IS NOT NULL` operators to check for null states; standard equality operators (`=`, `!=`) evaluate to unknown when comparing against `NULL`.
  * Handle potential `NULL` values in arithmetic operations using functions like `COALESCE()` to prevent calculated expressions from evaluating entirely to `NULL`.

---

### 39. What is the difference between SQL and NoSQL?
SQL databases are relational, structured, table-based systems that use strict predefined schemas and support ACID transactions (e.g., PostgreSQL, Oracle). NoSQL databases are non-relational systems designed with flexible, dynamic schemas optimized for unstructured or document-based data structures (e.g., MongoDB, Cassandra). We use SQL databases when high consistency, complex join queries, and strict transactions are required, whereas NoSQL is used for horizontal scaling, rapid schema iteration, and unstructured key-value or document storage.

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

**Best Practices:**
  * Choose SQL relational databases for core transactional business systems (like finance or inventory management) where data consistency and schema guarantees are paramount.
  * Choose NoSQL systems when dealing with massive real-time horizontal scaling, unstructured payload logs, or rapidly changing document schemas.

---

### 40. What are some common challenges when working with SQL databases?
Common challenges in SQL database engineering include handling query performance degradation on growing datasets, managing database locking and deadlocks during concurrent updates, designing index strategies, maintaining schema migration stability, and scaling databases vertically. We manage these challenges by applying query tuning, analyzing execution plans, monitoring connection pools, and utilizing partitioning or read-replica architectures.

```sql
-- Explain execution details to identify slow table scan issues
EXPLAIN ANALYZE 
SELECT * FROM tracking_logs WHERE search_code = 'ERR_404';
```

**Code Logic:**  
`EXPLAIN ANALYZE` commands the database planner to execute the query while capturing operational metrics, showing developers whether the search used an index lookup or suffered performance degradation from a full table scan.

**Terms Used:**
  * `EXPLAIN ANALYZE`: Performance profiling tool returning physical engine execution plans.

**Best Practices:**
  * Profile slow queries regularly using engine analysis tools (`EXPLAIN`) to catch unindexed scans and high-cost joins early.
  * Implement clear database schema migration controls and connection pool managers to handle operational scaling gracefully.

---

## Section 4: Intermediate SQL Questions

### 41. What is a Common Table Expression (CTE)?
A Common Table Expression (CTE) is a temporary named result set defined within the execution scope of a single `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement using the `WITH` keyword. We use CTEs to simplify complex multi-stage queries, replace unreadable nested subqueries, improve code maintenance, and execute recursive hierarchal queries cleanly.

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

**Best Practices:**
  * Use descriptive names for CTE blocks to clearly convey the intent of intermediate data transformations.
  * Break down sprawling, multi-level queries into chained CTE steps to enhance overall query readability and facilitate debugging.

---

### 42. What are window functions, and how do they differ from aggregate functions?
Window functions perform calculations across a set of related database rows (a window frame) using the `OVER()` clause while maintaining individual row identities in the final output. They differ from standard aggregate functions (`GROUP BY`) because standard aggregates collapse individual rows into summary single-row outputs, whereas window functions append calculated summary values directly to each uncollapsed original record row. We use window functions to compute running totals, moving averages, ranks, and lead/lag values without collapsing dataset details.

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

**Best Practices:**
  * Use `PARTITION BY` inside `OVER()` to logically group window computations without resorting to self-joins or nested subqueries.
  * Be mindful of performance when defining complex window frames over extremely large datasets; back partition and order columns with secondary indexes where appropriate.

---

### 43. What is the difference between RANK(), DENSE_RANK(), and ROW_NUMBER()?
`ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()` are window functions used to rank ordered dataset rows, differing specifically in how they handle tied values: `ROW_NUMBER()` assigns strictly incremental, unique integer ranks regardless of ties; `RANK()` assigns identical rank values to ties but skips subsequent rank numbers (e.g., 1, 1, 3); and `DENSE_RANK()` assigns identical ranks to ties without skipping subsequent sequence numbers (e.g., 1, 1, 2). We use these distinct functions based on whether ranking requires strict sequence numbers, rank gaps, or continuous dense ranks.

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

**Best Practices:**
  * Use `DENSE_RANK()` when identifying Top-N threshold values (like top 3 salary tiers) to avoid missing ranks caused by tie values.
  * Use `ROW_NUMBER()` when performing deterministic pagination or deduplication tasks where every output row must receive a distinct identifier.

---

### 44. What is a function in SQL?
An SQL function is a database object containing a predefined routine that accepts input parameters, performs dynamic computations or data transformations, and returns a single scalar value or tabular output. We use functions to encapsulate reusable scalar calculations (like mathematical rounding or string formatting) and automate repetitive transformation logic cleanly within SQL queries.

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

**Best Practices:**
  * Avoid wrapping indexed columns inside scalar functions in `WHERE` clauses (e.g., `WHERE UPPER(name) = 'ALICE'`), as this disables standard index usage; consider expression indexes instead.
  * Keep custom user-defined functions (UDFs) computationally lightweight to prevent row-by-row processing bottlenecks.

---

### 45. What types of SQL functions do you know?
SQL functions are categorized into Aggregate Functions (operating on multi-row sets to return a single summary value) and Scalar Functions (operating on individual single-row input values to return a single modified value per row). We use aggregate functions for summary analytics (`SUM`, `AVG`, `COUNT`) and scalar functions for individual row modifications (`SUBSTRING`, `ROUND`, `COALESCE`).

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

**Best Practices:**
  * Combine scalar functions inside aggregate functions purposefully, ensuring data type compatibility across nested calls.
  * Distinguish between scalar logic and aggregate logic clear in code to avoid invalid `GROUP BY` structural errors.

---

### 46. What SQL aggregate functions do you know?
Common SQL aggregate functions include `COUNT()` (counts row occurrences), `SUM()` (calculates numeric totals), `AVG()` (computes arithmetic means), `MIN()` (finds minimum values), and `MAX()` (finds maximum values). We use aggregate functions to perform summary statistics, generate business intelligence reports, and compute group metrics across database tables.

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

**Best Practices:**
  * Remember that standard aggregate functions (except `COUNT(*)`) automatically ignore `NULL` values during calculation.
  * Ensure every non-aggregated column in the `SELECT` list is explicitly included in the `GROUP BY` clause.

---

### 47. What SQL scalar functions do you know?
Common SQL scalar functions operate on individual row inputs and include String Functions (`UPPER`, `LOWER`, `CONCAT`, `SUBSTRING`, `TRIM`), Numeric Functions (`ROUND`, `ABS`, `CEIL`, `FLOOR`), Date/Time Functions (`NOW`, `CURRENT_DATE`, `DATEDIFF`, `EXTRACT`), and Conditional/Conversion Functions (`COALESCE`, `CAST`). We use scalar functions to format strings, handle numerical rounding, compute date differences, and cast data types per record.

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

**Best Practices:**
  * Standardize scalar function usage across application queries to ensure output data formatting remains consistent.
  * Be cautious with implicit data conversion inside scalar functions; explicitly use `CAST()` or `CONVERT()` to guarantee expected data types.

---

### 48. What are case manipulation functions in SQL?
Case manipulation functions are specialized string scalar functions used to adjust or convert character casing within text attributes—specifically `LOWER()` (converts text to lowercase), `UPPER()` (converts text to uppercase), and `INITCAP()` (capitalizes the first letter of each word). We use case manipulation functions to standardize string formatting, clean messy user inputs, and perform case-insensitive text matching comparisons.

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

**Best Practices:**
  * Apply `LOWER()` or `UPPER()` on string inputs during user registration or lookup processing to enforce casing consistency in application databases.
  * When doing case-insensitive filtering on large tables, evaluate using functional/expression indexes (or engine-native case-insensitive collations) to maintain index seek capabilities.

---

### 49. What are character manipulation functions in SQL?
Character manipulation functions are string scalar routines used to inspect, extract, modify, or measure text data fields—such as `CONCAT()` (joins strings), `SUBSTRING()` / `SUBSTR()` (extracts string parts), `LENGTH()` / `LEN()` (measures string length), `TRIM()` (removes whitespace), `REPLACE()` (swaps text patterns), and `INSTR()` (finds character positions). We use these functions to manipulate, cleanse, parse, and reformat textual data directly inside SQL expressions.

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

**Best Practices:**
  * Use `TRIM()` when processing raw incoming string data to eliminate leading or trailing whitespaces that cause failed joins or exact string match lookups.
  * Note that string index offsets can vary between database engines (most SQL dialects start string positions at 1, rather than 0).

---

### 50. What is the difference between local and global variables?
In procedural SQL programming (such as T-SQL or PL/PGSQL), local variables exist only within the specific block, batch, or stored procedure execution scope where they are declared. Global variables (or session/system variables) exist across the entire user session scope or system server instance level. We use local variables to capture temporary calculation states within procedures, whereas global/system variables track environment metadata (like execution errors, row counts, or server configurations).

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

**Best Practices:**
  * Keep local variables tightly scoped within specific script blocks to prevent unintended state leakage during procedure execution.
  * Avoid attempting to modify system-level global configuration variables inside standard query transactions.

---

### 51. What is the difference between SQL and PL/SQL?
SQL is a declarative data query language used to define, manipulate, and retrieve structured data via single set-based statements. PL/SQL (Procedural Language/SQL) is Oracle's proprietary procedural extension to SQL that integrates procedural programming concepts—such as conditional loops, branching logic, exception handling, and variable declarations—around standard SQL statements. We use SQL for set-based data retrieval and PL/SQL to write complex procedural routines, stored procedures, and triggers on Oracle platforms.

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

**Best Practices:**
  * Leverage set-based pure SQL query operations whenever possible before resorting to procedural PL/SQL loops, as set operations are heavily optimized by database query engines.
  * Include explicit error-handling blocks (`EXCEPTION`) inside PL/SQL procedures to manage transactional failures gracefully.

---

### 52. What is the difference between LEFT JOIN and LEFT OUTER JOIN?
There is zero functional or performance difference between `LEFT JOIN` and `LEFT OUTER JOIN`. `LEFT JOIN` is simply a syntactic shorthand keyword for `LEFT OUTER JOIN`. We use either term based on team syntactic coding standards to perform left outer joins, preserving all rows from the left table regardless of whether matching records exist in the right table.

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

**Best Practices:**
  * Adopt a consistent syntactic style across your team codebase—`LEFT JOIN` is widely preferred due to its brevity.
  * Always verify that filters applied to outer-joined tables are placed in the `ON` clause rather than the `WHERE` clause to avoid converting the query into an `INNER JOIN`.

---

### 53. What is indexing in SQL, and how does it improve performance?
Indexing is a database optimization technique where the engine creates an auxiliary data structure (typically a B-Tree) that maintains sorted key values pointing directly to physical row memory addresses on disk. We use indexing to improve performance because it transforms slow, high-cost full table scans ($O(N)$) into fast index seeks ($O(\log N)$), enabling the database engine to locate target search records almost instantly.

```sql
-- Accelerate customer lookup speed on order searches
CREATE INDEX idx_orders_customer ON customer_orders(customer_id);
```

**Code Logic:**  
This DDL statement constructs a secondary index structure on `customer_id`. When queries search for `WHERE customer_id = 88`, the engine jumps straight to target row locations using the index seek rather than reading every page of `customer_orders`.

**Terms Used:**
  * `CREATE INDEX`: Command constructing fast index lookup structures.
  * `Index Seek`: Quick traversal of index B-trees to target specific records directly.

**Best Practices:**
  * Index primary keys, foreign keys, and columns regularly queried in `WHERE`, `JOIN`, and `GROUP BY` clauses.
  * Measure query performance before and after index creation using execution plans (`EXPLAIN`) to confirm performance improvements.

---

### 54. What is a stored procedure, and how is it different from a function?
A stored procedure is a precompiled collection of procedural and SQL statements stored directly inside the database catalog to execute repetitive business workflows and administrative operations. It differs from a function because a stored procedure can execute data manipulation operations, modify database state, manage transaction control (`COMMIT`/`ROLLBACK`), return zero or multiple output values, and cannot be invoked directly within an inline `SELECT` statement. We use stored procedures to encapsulate business operations, enhance database security, and reduce application-to-database network round trips.

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

**Best Practices:**
  * Encapsulate complex or multi-table business mutations inside stored procedures to maintain centralized security and transaction boundaries.
  * Grant application users execution rights on stored procedures while restricting direct table access to prevent unauthorized data manipulation.

---

### 55. What is the default data ordering with the ORDER BY statement, and how do you change it?
The default sort ordering in an `ORDER BY` clause is Ascending (`ASC`), which sorts records from smallest to largest (1 to 9, A to Z, or earliest date to latest date). You change the sorting behavior to Descending order by explicitly adding the `DESC` keyword after the target column name. We use sorting modifiers to control how query output sequences are presented to end applications and users.

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

**Best Practices:**
  * Explicitly specify the desired direction (`ASC` or `DESC`) in query code to make sorting intent clear to other developers.
  * Be mindful of how your database engine handles `NULL` values when sorting (e.g., `NULLS FIRST` vs `NULLS LAST`).

---

### 56. What are SQL set operators?
SQL set operators are mathematical set commands used to combine complete result sets from two or more independent `SELECT` queries into a single output result set. Standard set operators include `UNION` (combines unique records), `UNION ALL` (combines all records including duplicates), `INTERSECT` (returns common records), and `EXCEPT` / `MINUS` (returns records present in the first query but absent in the second). We use set operators to consolidate uniform structured data across separate tables or conditional queries.

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

**Best Practices:**
  * Prefer `UNION ALL` over `UNION` whenever you know duplicates do not exist or when preserving duplicates is acceptable, as it avoids unnecessary sorting memory overhead.
  * Ensure all queries combined via set operators contain the exact same number of projected columns, matching data types, and identical ordering sequence.

---

### 57. What operator is used in the query for pattern matching?
The `LIKE` operator is used in SQL `WHERE` clauses to evaluate pattern-matching checks on text attributes using wildcard characters: `%` (representing zero or more dynamic characters) and `_` (representing exactly one single character). We use the `LIKE` operator to perform wildcards, text searches, handle fuzzy string evaluations, and locate substring patterns within character columns.

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

**Best Practices:**
  * Avoid starting `LIKE` search patterns with a leading wildcard (e.g., `LIKE '%term'`), as this forces full table/index scans; place wildcards at the end (`LIKE 'term%'`) to leverage indexes.
  * Use engine-native Full-Text Search indexing capabilities for advanced or large-scale document and string pattern searches.

---

### 58. What is the difference between a primary key and a unique key in SQL?
A Primary Key uniquely identifies each row in a table, completely forbids `NULL` values, automatically defines the physical clustered index order (in most engines), and a table can possess strictly only one Primary Key. A Unique Key enforces value uniqueness across columns, permits `NULL` values (depending on RDBMS), builds a non-clustered index, and a table can maintain multiple Unique Keys. We use Primary Keys for entity identification and Unique Keys for secondary uniqueness checks (like emails or usernames).

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

**Best Practices:**
  * Designate a simple, immutable surrogate key (like an auto-incrementing ID) as the `PRIMARY KEY`.
  * Use `UNIQUE` constraints for non-primary attributes that require domain duplicate protection across active business rows.

---

### 59. What is a SQL composite primary key?
A SQL composite primary key is a primary key that consists of two or more combined columns that collectively guarantee row uniqueness across a database table. We use composite primary keys in junction/bridge tables (which model Many-to-Many relationships) or weak entity sets where a single column is insufficient to uniquely identify a record, but the combined combination of target attributes guarantees absolute uniqueness.

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

**Best Practices:**
  * Limit composite keys to a minimal number of columns to prevent oversized secondary index structures.
  * Ensure the most selectively queried column in the composite key is defined first to maximize index performance for single-column searches.

---

### 60. What is the typical order of SQL clauses in a SELECT statement?
The required syntactic written order of clauses when composing a `SELECT` statement is: `SELECT` -> `FROM` -> `JOIN` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `ORDER BY` -> `LIMIT` / `TOP`. We write SQL statements using this explicit sequential hierarchy so the parser can properly compile the query structure without syntax errors.

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

**Best Practices:**
  * Maintain clean, consistent indentation for each major clause to ensure long queries are easy to read and review.
  * Keep clause dependencies aligned; for instance, ensure any column in `HAVING` is properly aggregated or present in `GROUP BY`.

---

### 61. In which order does the interpreter execute the common statements in the SELECT query?
Although written starting with `SELECT`, the logical execution processing order performed by the database engine parser is: 1. `FROM` / `JOIN` (data loading) -> 2. `WHERE` (row filtering) -> 3. `GROUP BY` (grouping) -> 4. `HAVING` (group filtering) -> 5. `SELECT` (projection & expression evaluation) -> 6. `DISTINCT` (deduplication) -> 7. `ORDER BY` (sorting) -> 8. `LIMIT` / `OFFSET` (paging). We understand this logical execution sequence to write valid queries—such as understanding why column aliases created in `SELECT` cannot be referenced in `WHERE` clauses.

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

**Best Practices:**
  * Do not attempt to filter by projected column aliases inside the `WHERE` clause, because `WHERE` executes before `SELECT` aliases are created.
  * Optimize early execution stages (`FROM` and `WHERE`) to drastically minimize the dataset volume passed into downstream execution steps like `GROUP BY` and `ORDER BY`.

---

### 62. What is a view in SQL?
A view in SQL is a named, virtual database table whose underlying contents are defined by the saved result set of an encapsulated `SELECT` query. A standard view does not store physical data rows on disk itself; instead, it dynamically executes its defining query against underlying base tables whenever queried. We use views to simplify complex queries, abstract complex table joins away from application developers, preserve security by restricting sensitive column exposures, and maintain backwards-compatible schema interfaces.

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

**Best Practices:**
  * Use views to encapsulate complex multi-table joins so client applications can query simplified interfaces.
  * Avoid layering multiple views on top of each other (nested views), as this can severely complicate query optimization and impair performance.

---

### 63. Can we create a view based on another view in SQL?
Yes, you can create a view based on an existing view; this is known as a nested view. The secondary view treats the initial view as if it were a standard base data table in its `FROM` clause. We use nested views to build modular data abstraction layers—such as taking a baseline "Active Users" view and creating specialized sub-views like "Active US Users".

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

**Best Practices:**
  * Keep view nesting depth minimal (ideally no more than 2 levels deep) to prevent execution plan bloat and poor performance.
  * Document base dependencies clearly so updates to primary views do not inadvertently break downstream nested views.

---

### 64. Can we still use a view if the original table is deleted?
No, you cannot query a standard view if its underlying base physical table is deleted or dropped. When a base table is dropped, the view becomes invalid or broken, because the query parser can no longer resolve the physical table and column references defined inside the view metadata. Querying an invalid view will result in a runtime execution error.

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

**Best Practices:**
  * Inspect view dependencies in your database catalog before dropping base tables.
  * Use database schema migration scripts that automatically drop or update dependent views prior to altering underlying base tables.

---

### 65. What types of SQL relationships do you know?
SQL database design supports three fundamental entity relationship structures: One-to-One (1:1), where a single record in Table A links to at most one record in Table B; One-to-Many (1:N), where a single record in Table A links to multiple records in Table B; and Many-to-Many (M:N), where multiple records in Table A link to multiple records in Table B via an intermediary junction table. We use these distinct relationship patterns to accurately model real-world business entity structures in normalized schemas.

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

**Best Practices:**
  * Always use dedicated junction tables with composite primary keys to resolve Many-to-Many relationships cleanly.
  * Define explicit foreign key constraints across all relational linkages to protect referential integrity.

---

### 66. What are the possible values of a BOOLEAN data field?
Under standard ANSI SQL 3-valued logic, a `BOOLEAN` data field can hold three possible state values: `TRUE`, `FALSE`, and `NULL` (representing an unknown or unassigned boolean state). We use `BOOLEAN` fields to record binary states (such as active status or flag indicators) while preserving `NULL` for missing or non-evaluated conditions.

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

**Best Practices:**
  * Use `NOT NULL DEFAULT FALSE` on boolean columns when missing unknown states are invalid for business logic.
  * Remember that logical boolean checks against `NULL` evaluate to `UNKNOWN` in standard SQL conditional evaluations.

---

### 67. What is normalization in SQL?
Normalization is a systematic database design technique used to organize table columns and relationships to minimize data redundancy, eliminate update/insert/delete anomalies, and ensure data dependencies make logical sense. It involves decomposing bloated tables into smaller, cohesive entities across standardized Normal Forms (1NF, 2NF, 3NF, BCNF). We normalize schemas to maintain maximum data consistency, lower storage overhead, and streamline database update operations.

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

**Best Practices:**
  * Aim for 3rd Normal Form (3NF) as standard baseline design for transactional (OLTP) enterprise applications.
  * Balance normalization principles with practical read access patterns to avoid overly complex schema fragmentation.

---

### 68. What is denormalization in SQL?
Denormalization is the deliberate strategy of introducing redundant data or combining normalized tables into fewer, consolidated tables to optimize read performance and simplify analytical reporting. We use denormalization primarily in Data Warehouses and Analytical Processing (OLAP) environments to eliminate computationally expensive `JOIN` operations and speed up heavy aggregation queries on massive datasets.

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

**Best Practices:**
  * Reserve denormalization for heavy read-mostly systems (like reporting warehouses) rather than core transactional systems.
  * Manage redundant data updates carefully (using triggers or batch ETL pipelines) to keep denormalized values synchronized.

---

### 69. What is the difference between renaming a column and giving an alias to it?
Renaming a column alters the permanent catalog metadata definition of the table structure using Data Definition Language (`ALTER TABLE ... RENAME COLUMN`), permanently changing the database schema. Assigning an alias (`SELECT column AS alias_name`) provides a temporary display label for that column strictly within the runtime execution scope of a single query, leaving the persistent database schema unchanged. We use renaming for structural schema refactoring and aliases for query output formatting.

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

**Best Practices:**
  * Coordinate permanent column renames with application development teams to prevent breaking active application code queries.
  * Use aliases liberally in reporting queries to give clean, meaningful header names to computed expressions and raw fields.

---

### 70. What is the difference between nested and correlated subqueries?
A standard nested subquery executes independently of the outer main query once, passing its static computed result set back to the outer query for filtering. A correlated subquery references columns from the outer query directly, requiring the database engine to execute the subquery repeatedly for every single candidate row evaluated by the outer query. We use correlated subqueries for contextual row-by-row comparative checks, though independent subqueries are generally more performant.

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

**Best Practices:**
  * Rewrite heavy correlated subqueries as `JOIN` operations or window functions (`AVG() OVER(...)`) to allow the query optimizer to process datasets in parallel set operations.
  * Ensure correlation key columns in subqueries are backed by indexes to speed up row-by-row lookups.

---

### 71. What is the difference between clustered and non-clustered indexes?
A Clustered Index determines the actual physical, sequential storage order of data rows on the disk (a table can have strictly only one clustered index, usually assigned to the Primary Key). A Non-Clustered Index creates a separate, auxiliary lookup structure containing ordered key values paired with row pointers that reference the underlying physical data pages (a table can maintain multiple non-clustered indexes). We use clustered indexes for range scans on primary keys and non-clustered indexes to accelerate secondary filter lookups.

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

**Best Practices:**
  * Choose a monotonically increasing attribute (like an auto-incrementing integer ID) as your clustered index to prevent page splitting on disk.
  * Create non-clustered indexes selectively on frequently filtered or joined secondary columns to balance query speed with write efficiency.

---

### 72. What is the CASE() function?
The `CASE` function is a conditional scalar expression in SQL that allows developers to embed IF-THEN-ELSE evaluation logic directly within queries. It evaluates a sequence of conditions (`WHEN condition THEN result`) and returns a corresponding output value when a condition evaluates to true, optionally falling back to an `ELSE` default value. We use `CASE` statements to perform inline value transformations, categorize metrics, map status codes, and drive conditional aggregations.

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

**Best Practices:**
  * Always include an explicit `ELSE` clause in `CASE` expressions to prevent unhandled cases from evaluating to unexpected `NULL` values.
  * Combine `CASE` inside aggregate functions (`SUM(CASE WHEN ... THEN 1 ELSE 0 END)`) to perform pivot calculations and conditional metrics efficiently.

---

### 73. What is the difference between the DELETE and TRUNCATE statements?
`DELETE` is a Data Manipulation Language (DML) command that removes specific target rows one by one using a `WHERE` clause, logs every individual row deletion in transaction logs, triggers table delete events, and can be safely rolled back within a transaction. `TRUNCATE` is a Data Definition Language (DDL) command that instantly empties an entire table by deallocating its physical data storage pages, ignores `WHERE` clauses, logs minimal page deallocations, resets identity seeds, and executes significantly faster. We use `DELETE` for selective row removal and `TRUNCATE` for rapid total data wipes.

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

**Best Practices:**
  * Use `DELETE` when you need fine-grained conditional row removal (`WHERE`) or need to trigger cascading delete mechanisms.
  * Use `TRUNCATE` when clearing staging or temporary data tables where execution speed is paramount and triggers are not required.

---

### 74. What is the difference between the DROP and TRUNCATE statements?
`TRUNCATE` removes all stored data rows from a table while leaving the complete physical schema structure, column definitions, constraints, and indexes intact for future data entry. `DROP` destroys the entire table object permanently—erasing all contained data rows, index definitions, constraint triggers, and the underlying schema entry from the system catalog entirely. We use `TRUNCATE` to clear data while keeping structures, and `DROP` to decommission tables completely.

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

**Best Practices:**
  * Verify whether dependent views or stored procedures exist before executing `DROP TABLE` to prevent breaking application features.
  * Use `TRUNCATE` when reusing baseline table structures across iterative ETL pipeline runs.

---

### 75. What is the difference between the HAVING and WHERE statements?
The `WHERE` clause filters individual row records from source tables *before* any grouping or aggregate calculations take place, and it cannot evaluate aggregate functions directly. The `HAVING` clause filters aggregated group summaries *after* the `GROUP BY` clause has processed input records, and it explicitly evaluates aggregate expressions (`COUNT()`, `AVG()`). We use `WHERE` for row-level conditional filters and `HAVING` for group summary metric filters.

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

**Best Practices:**
  * Apply non-aggregate row filters in the `WHERE` clause to eliminate non-matching records as early as possible before grouping overhead occurs.
  * Restrict the `HAVING` clause exclusively to conditions that evaluate aggregate function outputs (such as `SUM(amount) > 1000`).

---

### 76. How do you add a record to a table?
You add a new record row to a table using the `INSERT INTO` Data Manipulation Language (DML) statement, specifying the target table name, target column fields, and matching literal row values inside a `VALUES()` clause. We use `INSERT INTO` statements to record new operational business entities—such as registering new accounts, capturing transactions, or populating logs.

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

**Best Practices:**
  * Explicitly define destination column names in the `INSERT` clause rather than relying on implicit positional column order to insulate code against table structural changes.
  * Use parameterized queries or prepared statements when passing application variable values into `INSERT` statements to prevent SQL Injection vulnerabilities.

---

### 77. How do you delete a record from a table?
You delete specific records from a table using the `DELETE FROM` Data Manipulation Language (DML) statement combined with a `WHERE` clause specifying matching search criteria. We use target delete statements to purge expired records, remove invalid entries, and fulfill data deletion requests while leaving non-matching table records untouched.

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

**Best Practices:**
  * Always execute a target `DELETE` within a transaction block during manual database maintenance so you can issue a `ROLLBACK` if wrong records are matched.
  * Filter target records using primary key identifiers (`WHERE user_id = X`) to guarantee only intended target records are removed.

---

### 78. How do you add a column to a table?
You add a new column field to an existing table schema using the `ALTER TABLE` Data Definition Language (DDL) command combined with the `ADD` clause, specifying the column name, data type, and optional constraints. We use this structural command to evolve live database schemas over time as application requirements change and expand.

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

**Best Practices:**
  * When adding new columns to populated tables, define them as nullable or set a `DEFAULT` value to prevent migration errors across existing rows.
  * Execute column additions during scheduled maintenance windows on high-traffic production databases to prevent table-locking latency spikes.

---

### 79. How do you rename a column of a table?
You permanently rename an existing column in a table schema using the `ALTER TABLE` command combined with `RENAME COLUMN ... TO ...` (syntax varies slightly across dialects like PostgreSQL, MySQL, and SQL Server). We use column renaming commands during database refactoring to improve naming consistency, correct typos, or adjust attributes to reflect updated business domains.

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

**Best Practices:**
  * Search application codebase repositories for all references to the old column name before renaming it in the database schema.
  * Update dependent views, stored procedures, and triggers that reference the renamed column to prevent runtime query failures.

---

### 80. How do you delete a column from a table?
You delete a column permanently from a table using the `ALTER TABLE` Data Definition Language (DDL) command combined with the `DROP COLUMN` clause. We use column dropping operations to decommission obsolete attributes, clean up deprecated schemas, and recover physical disk storage space occupied by redundant data.

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

**Best Practices:**
  * Take a full schema and data backup prior to dropping columns from production database tables.
  * Confirm that no foreign key constraints, indexes, views, or active application queries depend on the target column before dropping it.

---

### 81. How do you select all even or all odd records in a table?
You isolate even or odd records by applying the modulo arithmetic operator (`%` or `MOD()`) on a numerical ID or sequential row number column within the query `WHERE` clause (checking `id % 2 = 0` for even records, and `id % 2 != 0` for odd records). We use modulo filtering operations in batch processing, data sampling, and distributed task load-balancing algorithms.

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

**Best Practices:**
  * Rely on modulo calculations on monotonically increasing primary keys or explicit `ROW_NUMBER()` window outputs rather than assuming natural physical row storage positions.
  * Be aware that modulo operations applied directly on indexed columns force full table scans; use calculated split ranges for heavy parallel batch queries.

---

### 82. How to prevent duplicate records when making a query?
You prevent duplicate records in query result sets by incorporating the `DISTINCT` keyword in your `SELECT` projection list or by grouping result records using a `GROUP BY` clause. We use deduplication query logic to consolidate repeating data, clean analytical outputs, and produce explicit unique key lists from raw relational tables containing redundant row entries.

```sql
-- Deduplicate output list of customer locations
SELECT DISTINCT location_city 
FROM customer_profiles;
```

**Code Logic:**  
The engine executes `SELECT DISTINCT`, scanning `customer_profiles` for `location_city` values, stripping recurring duplicates, and outputting a single unique representative instance for every discovered city string.

**Terms Used:**
  * `DISTINCT`: Keyword modifier eliminating duplicate rows from output sets.

**Best Practices:**
  * Identify why duplicates exist at the schema tier first; if data duplication is caused by incorrect join conditions, fix the join logic rather than masking it with `DISTINCT`.
  * Prefer `GROUP BY` when deduplicating if you also need to calculate aggregate summary metrics (`COUNT`, `SUM`) alongside unique field outputs.

---

### 83. How do you insert many rows in a table?
You insert multiple row records efficiently into a table by passing a comma-separated list of multiple value tuples inside a single multi-row `INSERT INTO ... VALUES (), (), ()` statement block. We use multi-row bulk insertions to minimize database round-trip network latency, reduce transaction log overhead, and drastically speed up batch data loading routines.

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

**Best Practices:**
  * Batch inserts into reasonable chunk sizes (e.g., 1,000 to 5,000 rows per statement) to optimize network packet usage without overwhelming server memory.
  * Wrap bulk multi-row operations within an explicit single transaction (`BEGIN TRANSACTION` ... `COMMIT`) to maximize write throughput.

---

### 84. How do you find the nth highest value in a column of a table?
You locate the Nth highest value in a column using the `DENSE_RANK()` window function inside a Common Table Expression (CTE) and filtering for `WHERE rank = N`, or by sorting data `ORDER BY column DESC` combined with dialect offset pagination (`LIMIT 1 OFFSET N-1`). We use the `DENSE_RANK()` method because it properly handles tied duplicate values without skipping rank positions.

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

**Best Practices:**
  * Prefer `DENSE_RANK()` over basic `LIMIT/OFFSET` hacks to correctly handle scenarios where multiple records share duplicate tie values for the target rank.
  * Ensure the evaluated column (e.g., `salary`) is properly indexed to make sorting and rank evaluations fast.

---

### 85. How do you find the values in a text column of a table that start with a certain letter?
You find text entries starting with a specific letter by using the `LIKE` pattern matching operator in a `WHERE` clause, passing the target starting character followed immediately by the `%` wildcard (e.g., `WHERE column_name LIKE 'A%'`). We use prefix pattern matching to drive search auto-completes, alphabetical sorting filters, and categorical text lookups.

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

**Best Practices:**
  * Place the wildcard at the end (`'A%'`) so the query engine can utilize standard column indexes via range seeks.
  * Use `LOWER()` or case-insensitive collations if search inputs must match both uppercase and lowercase starting characters.

---

### 86. How do you find the last id in a table?
You find the highest or last allocated ID in a table using the `MAX(id)` aggregate function or by ordering records `ORDER BY id DESC` capped with a single-row limit (`LIMIT 1`). We use last ID lookups to verify identity sequence allocations, inspect recently inserted transactional records, and evaluate table key bounds.

```sql
-- Calculate peak primary key ID allocated in sales table
SELECT MAX(order_id) AS last_order_id 
FROM sales_orders;
```

**Code Logic:**  
The aggregate function `MAX(order_id)` scans the primary index key values in `sales_orders` and instantly extracts the largest numerical identifier currently assigned in the table.

**Terms Used:**
  * `MAX`: Aggregate function evaluating the highest numerical or scalar value in a column.

**Best Practices:**
  * Rely on `MAX(id)` on indexed Primary Key columns, as the engine can resolve the maximum value instantly via index boundary inspection ($O(1)$ complexity).
  * In highly concurrent environments, avoid using `MAX(id)` to guess future foreign key values; use native returning clauses (`RETURNING id` or `SCOPE_IDENTITY()`) during insertion instead.

---

### 87. How to select random rows from a table?
You select random rows from a table by ordering query results by random number generator functions (`ORDER BY RANDOM()` in PostgreSQL/SQLite, `ORDER BY RAND()` in MySQL, or `ORDER BY NEWID()` in SQL Server) and bounding output volume using `LIMIT N`. We use random row sampling for audit checks, A/B test sampling, randomized quiz generations, and statistical QA inspections.

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

**Best Practices:**
  * Avoid using `ORDER BY RANDOM()` on extremely large tables (millions of rows), as it forces a full table scan and expensive sort in memory; use indexed primary key offset sampling instead.
  * Use dedicated sample clauses (like `TABLESAMPLE` in PostgreSQL/SQL Server) for high-performance statistical sampling on large disk datasets.

---

## Section 5: Scenario-Based SQL Questions

### 88. How do you find and remove duplicate records from a table?
You locate duplicates using `GROUP BY` paired with `HAVING COUNT(*) > 1`. You safely purge duplicates while retaining one original row by wrapping the table in a Common Table Expression (CTE), using `ROW_NUMBER() OVER (PARTITION BY [duplicate_columns] ORDER BY primary_key)` to number duplicate instances, and executing a `DELETE` where `row_num > 1`. We use this pattern to clean up corrupted data pipelines and enforce entity deduplication safety.

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

**Best Practices:**
  * Test row partition results using a `SELECT` query before executing the final `DELETE` operation to verify that correct primary records are retained.
  * Add a `UNIQUE` constraint or index on the deduplicated columns after cleaning to permanently prevent duplicate re-entry.

---

### 89. How do you calculate a running total (cumulative sum)?
You calculate a cumulative running total using the `SUM()` aggregate function evaluated as a window function paired with `OVER (ORDER BY transaction_date)`. We use running total calculations across financial ledgers, inventory tracking systems, and executive dashboards to monitor metric progression continuously over time.

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

**Best Practices:**
  * Include a unique secondary tie-breaker column (e.g., `ORDER BY transaction_date, transaction_id`) to ensure deterministic running total outputs when duplicate dates occur.
  * Use `PARTITION BY` inside `OVER()` if running totals must reset periodically per entity (e.g., calculating running sales totals per individual customer).

---

### 90. How do you find employees who earn more than the average salary in their department?
You identify employees earning above their localized department average using a correlated subquery that compares individual row salaries against subquery department averages, or by using a CTE/window function (`AVG(salary) OVER (PARTITION BY dept_id)`) to calculate department averages and filtering rows where `salary > dept_avg`. We use these comparative contextual queries to analyze compensation parity, isolate outliers, and drive HR metrics.

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

**Best Practices:**
  * Prefer the CTE or window function approach over correlated subqueries on large tables to compute department averages in a single pass rather than re-calculating per row.
  * Ensure the department ID filter column (`dept_id`) is indexed to maintain fast subquery lookup speeds.

---

### 91. How do you find gaps in a sequence of numbers (e.g., missing invoice numbers)?
You identify gaps in a sequential numeric sequence using the `LEAD()` window function to inspect the next sequential ID row value (`LEAD(invoice_id) OVER (ORDER BY invoice_id)`), and checking for gap conditions where `next_id - current_id > 1`. We use sequence gap analysis to detect missing invoice records, identify skipped audit logs, and isolate missing transactional sequences in accounting systems.

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

**Best Practices:**
  * Back sequence evaluation columns (`invoice_id`) with a primary key or unique index to make window navigation efficient.
  * Handle edge cases for missing numbers at the very start (ID 1) or end of sequence ranges explicitly if sequence bounds start at fixed values.

---

### 92. How do you find customers who made purchases in consecutive months?
You find consecutive monthly purchases by normalizing order dates to monthly boundaries (`DATE_TRUNC('month', order_date)`), using the `LAG()` window function partitioned by customer to pull each customer's previous purchase month, and filtering for records where the current purchase month is exactly one month after the previous month (`purchase_month = prev_month + INTERVAL '1 month'`). We use consecutive activity detection to analyze customer retention, user engagement cohorts, and subscription renewals.

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

**Best Practices:**
  * Distinctly group purchase dates by month first (`SELECT DISTINCT customer_id, month`) before applying `LAG()` to avoid multiple purchases within the same month being mistaken for non-consecutive activity.
  * Ensure date arithmetic operators match your database dialect syntax (e.g., `INTERVAL '1 month'` in PostgreSQL vs `DATEADD()` in SQL Server).

---

### 93. How do you pivot data from rows to columns?
You pivot vertical row data into horizontal display columns using conditional aggregation (`SUM(CASE WHEN category = 'X' THEN amount ELSE 0 END)`) grouped by entity, or by using native dialect `PIVOT` clauses. We use data pivoting to convert granular transaction rows into compact tabular cross-tab summaries, quarterly reports, and executive dashboards.

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

**Best Practices:**
  * Use conditional aggregation (`SUM(CASE...)`) for maximum cross-database query portability across different SQL engines.
  * Ensure `ELSE 0` or `ELSE NULL` is specified inside conditional aggregate expressions to manage non-matching rows correctly.

---

### 94. How do you find the top 3 products by sales in each category?
You calculate Top-N items per category using a Common Table Expression (CTE) that applies `DENSE_RANK() OVER (PARTITION BY category_id ORDER BY total_sales DESC)` to rank products within each category partition, and then filtering the outer query for `WHERE rnk <= 3`. We use Top-N partitioned queries for category leaderboard tracking, inventory prioritization, and localized sales reporting.

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

**Best Practices:**
  * Use `DENSE_RANK()` instead of `ROW_NUMBER()` if you want to include all products that tie for 3rd place within a category.
  * Back partition and ordering columns (`category_id`, `total_sales`) with composite indexes to speed up window calculations on large catalogs.

---

### 95. What are ACID properties in database transactions?
ACID represents four fundamental transactional principles that guarantee database reliability: **Atomicity** (all operations in a transaction succeed, or all are rolled back as a single unit), **Consistency** (transactions transition the database from one valid state to another, respecting all constraints), **Isolation** (concurrent transactions execute independently without uncommitted cross-interference), and **Durability** (committed changes persist permanently, even through system power crashes). We depend on ACID properties to ensure absolute financial and operational data correctness.

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

**Best Practices:**
  * Keep transactions as short and concise as possible to minimize row locking time and optimize throughput.
  * Always include error handling (`TRY...CATCH` or exception blocks) to issue explicit `ROLLBACK` commands when transactional updates fail.

---

### 96. What is a deadlock, and how do you prevent it?
A deadlock occurs in multi-user concurrent database environments when two or more transactions hold exclusive locks on resources that the other transactions need, creating a perpetual cyclic dependency where neither transaction can proceed. You prevent deadlocks by ensuring all application code accesses and updates database tables in a strictly consistent alphabetical or numerical sequence, keeping transaction execution times minimal, utilizing appropriate isolation levels, and indexing lookup keys.

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

**Best Practices:**
  * Access multiple tables and rows in the exact same deterministic order across all application services and procedures.
  * Implement retry logic in application database layers to catch and gracefully re-run transactions killed by database deadlock detectors.

---

### 97. How do you optimize a slow-running SQL query?
You optimize slow SQL queries by reviewing physical execution plans (`EXPLAIN ANALYZE`), creating targeted indexes on `WHERE`, `JOIN`, and `ORDER BY` columns, replacing expensive `SELECT *` statements with explicit column selection, eliminating unindexed leading wildcards (`LIKE '%term'`), replacing correlated subqueries with `JOIN` operations or CTEs, and avoiding scalar functions on indexed filtering columns. We tune queries to reduce CPU, disk I/O, and memory overhead across production database servers.

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

**Best Practices:**
  * Always profile slow queries using `EXPLAIN` to identify whether latency is driven by full table scans, high-cost joins, or unindexed sorts.
  * Benchmark performance improvements in staging environments with realistic data volumes before applying optimizations to production.

---

### 98. How do you handle NULL values in calculations and comparisons?
You manage `NULL` values in calculations and conditional checks by using explicit null-handling scalar functions such as `COALESCE(val, fallback)` or dialect equivalents (`IFNULL()`, `NVL()`), and by utilizing explicit boolean operators `IS NULL` or `IS NOT NULL` for comparisons. We handle `NULL` values explicitly to prevent mathematical expressions from evaluating entirely to `NULL` and to avoid missing record matches caused by three-valued logic.

```sql
-- Replace NULL values with zero fallback during compensation arithmetic
SELECT emp_name, salary + COALESCE(bonus_amount, 0.00) AS total_compensation 
FROM company_staff;
```

**Code Logic:**  
`COALESCE(bonus_amount, 0.00)` inspects `bonus_amount`. If the field contains a valid number, it outputs that number; if the field is `NULL`, it outputs `0.00`, preventing addition operations from resolving to `NULL`.

**Terms Used:**
  * `COALESCE`: Scalar function returning the first non-null argument in its parameter list.

**Best Practices:**
  * Prefer standard ANSI `COALESCE()` over vendor-specific functions (like `NVL` or `IFNULL`) for superior code portability across database systems.
  * Always wrap nullable numeric calculation columns with `COALESCE()` to guarantee valid numeric output metrics.

---

### 99. How do you find the longest streak of consecutive login days for a user?
You find consecutive login streaks using the "Gaps and Islands" algorithm pattern: 1. Isolate distinct user login dates in a CTE; 2. Subtract an incremental row number from the login date (`login_date - ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date)`) to create a constant date group marker for unbroken consecutive runs; 3. Group by user and group marker to count streak lengths (`COUNT(*)`); 4. Take the `MAX(streak_length)` per user. We use streak analysis for user engagement gamification, activity tracking, and retention analysis.

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

**Best Practices:**
  * Always deduplicate timestamp entries to date-only grains (`CAST(login_time AS DATE)`) first so multiple logins on the same day do not disrupt sequence calculations.
  * Index target user and date timestamp columns (`user_id`, `login_time`) to keep window partitioning fast on large log tables.

