# MySQL Learning Summary

## 1. MySQL Statements

SQL commands are instructions to communicate with the database. They perform tasks like creating tables, inserting data, modifying structure, and querying data.

---

## 2. SQL Command Categories

### A. Data Definition Language (DDL)

Used to define or modify database structure. **Auto-committed** (changes are permanent).

**Key Commands:**

- **CREATE** – create database/table
- **DROP** – delete table or database
- **ALTER** – modify table structure
- **TRUNCATE** – delete all rows and free space
- **RENAME** – rename a table

**Example:**
```sql
CREATE DATABASE company;
CREATE TABLE employee (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);
ALTER TABLE employee ADD COLUMN salary DECIMAL(10,2);
DROP TABLE employee;
TRUNCATE TABLE employee;
RENAME TABLE employee TO staff;
```

---

### B. Data Manipulation Language (DML)

Used to modify data in tables. **Not auto-committed** (can be rolled back).

**Key Commands:**

- **INSERT** – add new records
- **UPDATE** – modify existing records
- **DELETE** – remove records

**Example:**
```sql
INSERT INTO employee (id, name) VALUES (1, 'John');
UPDATE employee SET salary = 50000 WHERE id = 1;
DELETE FROM employee WHERE id = 1;
```

---

### C. Data Control Language (DCL)

Manage user privileges and access.

**Key Commands:**

- **GRANT** – give privileges
- **REVOKE** – remove privileges

**Example:**
```sql
GRANT SELECT, INSERT ON database.* TO 'user'@'localhost';
REVOKE INSERT ON database.* FROM 'user'@'localhost';
```

---

### D. Transaction Control Language (TCL)

Manage transactions for DML operations.

**Commands:**

- **COMMIT** – save changes permanently
- **ROLLBACK** – undo changes
- **SAVEPOINT** – set points within a transaction

**Example:**
```sql
START TRANSACTION;
UPDATE employee SET salary = 60000 WHERE id = 1;
SAVEPOINT sp1;
DELETE FROM employee WHERE id = 2;
ROLLBACK TO sp1;
COMMIT;
```

---

### E. Data Query Language (DQL)

Used to fetch data from database. **SELECT** is the primary command.

Supports clauses like **WHERE**, **GROUP BY**, **HAVING**, **ORDER BY**.

**Example:**
```sql
SELECT * FROM employee WHERE salary > 50000;
SELECT department, AVG(salary) FROM employee GROUP BY department;
SELECT * FROM employee ORDER BY salary DESC;
```

---

## 3. MySQL Data Types

### Numeric

- **TINYINT, SMALLINT, MEDIUMINT, INT, BIGINT** – integer types
- **FLOAT, DOUBLE, DECIMAL** – floating-point types
- **BIT, BOOL, BOOLEAN** – special numeric types

**Example:**
```sql
CREATE TABLE numbers (
    small_num TINYINT,
    regular_num INT,
    big_num BIGINT,
    price DECIMAL(10,2),
    percentage FLOAT
);
```

---

### Date & Time

- **DATE** – YYYY-MM-DD
- **TIME** – HH:MM:SS
- **DATETIME** – YYYY-MM-DD HH:MM:SS
- **TIMESTAMP** – YYYY-MM-DD HH:MM:SS (auto-updates)
- **YEAR** – YYYY

**Example:**
```sql
CREATE TABLE events (
    event_date DATE,
    event_time TIME,
    created_at DATETIME,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

### String

- **CHAR(size)** – fixed length (0-255)
- **VARCHAR(size)** – variable length (0-65535)
- **TEXT, TINYTEXT, MEDIUMTEXT, LONGTEXT** – large text
- **BINARY(size)** – binary data
- **ENUM** – predefined values

**Example:**
```sql
CREATE TABLE users (
    username VARCHAR(50),
    bio TEXT,
    gender ENUM('male', 'female', 'other'),
    country CHAR(2)
);
```

---

## 4. Table Constraints

| Constraint | Description |
|------------|-------------|
| **PRIMARY KEY** | Uniquely identifies a record |
| **FOREIGN KEY** | Links two tables, maintains referential integrity |
| **NOT NULL** | Column cannot be empty |
| **UNIQUE** | All values in column must be distinct |
| **DEFAULT** | Assigns default value if none provided |
| **CHECK** | Ensures column values meet specific condition |
| **AUTO_INCREMENT** | Automatically increments numeric ID |

**Example:**
```sql
CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 18),
    department_id INT,
    salary DECIMAL(10,2) DEFAULT 30000.00,
    FOREIGN KEY (department_id) REFERENCES department(id)
);
```

---

## 5. SELECT & WHERE Clause

Fetch data with conditions.

### Operators

| Operator | Description | Example |
|----------|-------------|---------|
| **AND, OR** | Combine conditions | `WHERE age > 25 AND salary > 50000` |
| **IN, NOT IN** | Match values from list | `WHERE city IN ('Mumbai', 'Delhi')` |
| **BETWEEN** | Values within a range | `WHERE salary BETWEEN 40000 AND 60000` |
| **IS NULL / IS NOT NULL** | Check for null values | `WHERE email IS NOT NULL` |
| **LIKE** | Pattern matching | `WHERE name LIKE 'A%'` |

### LIKE Patterns

- `%` – any number of characters
- `_` – single character

**Examples:**
```sql
-- Names starting with 'A'
SELECT * FROM employee WHERE name LIKE 'A%';

-- Names ending with 'n'
SELECT * FROM employee WHERE name LIKE '%n';

-- Names with 'oh' in between
SELECT * FROM employee WHERE name LIKE '%oh%';

-- Names with exactly 4 characters
SELECT * FROM employee WHERE name LIKE '____';

-- Remove duplicates
SELECT DISTINCT city FROM employee;
```

---

## 6. Aggregate Functions

Perform calculations on multiple rows.

| Function | Description | Example |
|----------|-------------|---------|
| **COUNT()** | Number of rows | `SELECT COUNT(*) FROM employee` |
| **SUM()** | Sum of values | `SELECT SUM(salary) FROM employee` |
| **AVG()** | Average of values | `SELECT AVG(salary) FROM employee` |
| **MIN()** | Minimum value | `SELECT MIN(salary) FROM employee` |
| **MAX()** | Maximum value | `SELECT MAX(salary) FROM employee` |

**Examples:**
```sql
-- Total employees
SELECT COUNT(*) AS total_employees FROM employee;

-- Total payroll
SELECT SUM(salary) AS total_payroll FROM employee;

-- Average salary
SELECT AVG(salary) AS avg_salary FROM employee;

-- Salary range
SELECT MIN(salary) AS min_salary, MAX(salary) AS max_salary FROM employee;
```

---

## 7. ORDER BY, GROUP BY & HAVING

### ORDER BY

Sort results in ascending (`ASC`) or descending (`DESC`) order.

```sql
-- Sort by salary (highest first)
SELECT * FROM employee ORDER BY salary DESC;

-- Sort by multiple columns
SELECT * FROM employee ORDER BY department, salary DESC;
```

### GROUP BY

Group rows with same values.

```sql
-- Count employees per department
SELECT department, COUNT(*) AS emp_count 
FROM employee 
GROUP BY department;

-- Average salary per department
SELECT department, AVG(salary) AS avg_salary 
FROM employee 
GROUP BY department;
```

### HAVING

Filter groups after aggregation (used with GROUP BY).

```sql
-- Departments with more than 5 employees
SELECT department, COUNT(*) AS emp_count 
FROM employee 
GROUP BY department 
HAVING COUNT(*) > 5;

-- Departments with average salary > 50000
SELECT department, AVG(salary) AS avg_salary 
FROM employee 
GROUP BY department 
HAVING AVG(salary) > 50000;
```

**Difference:**
- **WHERE** filters individual rows before grouping
- **HAVING** filters groups after aggregation

---

## 8. Joins

Combine rows from multiple tables based on related columns.

### INNER JOIN

Returns only matching rows in both tables.

```sql
SELECT e.name, d.dept_name
FROM employee e
INNER JOIN department d ON e.dept_id = d.id;
```

### LEFT JOIN (LEFT OUTER JOIN)

Returns all rows from left table + matching right table rows.

```sql
SELECT e.name, d.dept_name
FROM employee e
LEFT JOIN department d ON e.dept_id = d.id;
```

### RIGHT JOIN (RIGHT OUTER JOIN)

Returns all rows from right table + matching left table rows.

```sql
SELECT e.name, d.dept_name
FROM employee e
RIGHT JOIN department d ON e.dept_id = d.id;
```

### CROSS JOIN

Returns Cartesian product of two tables (all possible combinations).

```sql
SELECT e.name, p.project_name
FROM employee e
CROSS JOIN project p;
```

### SELF JOIN

Join table with itself to compare rows within the same table.

```sql
-- Find employees earning more than each other
SELECT e1.name AS employee, e2.name AS higher_paid
FROM employee e1
JOIN employee e2 ON e1.salary < e2.salary;
```

### Visual Join Summary

| Join Type | Returns |
|-----------|---------|
| **INNER JOIN** | Only matching rows from both tables |
| **LEFT JOIN** | All from left + matching from right |
| **RIGHT JOIN** | All from right + matching from left |
| **CROSS JOIN** | All possible combinations |
| **SELF JOIN** | Compare rows within same table |

---

## 9. Subqueries

A query nested inside another query.

### Types of Subqueries

**1. Scalar Subquery** (returns single value)
```sql
-- Employees earning more than average
SELECT name, salary
FROM employee
WHERE salary > (SELECT AVG(salary) FROM employee);
```

**2. Row Subquery** (returns single row)
```sql
SELECT * FROM employee
WHERE (dept_id, salary) = (SELECT dept_id, MAX(salary) FROM employee);
```

**3. Table Subquery** (returns multiple rows)
```sql
-- Employees in Mumbai departments
SELECT name
FROM employee
WHERE dept_id IN (SELECT id FROM department WHERE city = 'Mumbai');
```

### Subquery in Different Clauses

**WHERE Clause:**
```sql
SELECT name FROM employee
WHERE salary = (SELECT MAX(salary) FROM employee);
```

**FROM Clause:**
```sql
SELECT dept_name, avg_sal
FROM (SELECT dept_id, AVG(salary) AS avg_sal FROM employee GROUP BY dept_id) AS dept_avg
JOIN department ON dept_avg.dept_id = department.id;
```

**SELECT Clause:**
```sql
SELECT name, salary, 
       (SELECT AVG(salary) FROM employee) AS company_avg
FROM employee;
```

---

## 10. Key Learnings

✅ Learned how to create, modify, and query tables  
✅ Understood different SQL command categories (DDL, DML, DCL, TCL, DQL)  
✅ Practiced joins, subqueries, and aggregate functions  
✅ Explored MySQL data types, constraints, and transaction management  
✅ Gained hands-on knowledge of filtering, sorting, and pattern matching in queries  
✅ Mastered the difference between WHERE and HAVING  
✅ Learned to combine data from multiple tables using various join types  
✅ Understood how to use subqueries for complex data retrieval  

---

## Quick Reference Commands

```sql
-- Database Operations
CREATE DATABASE db_name;
DROP DATABASE db_name;
USE db_name;
SHOW DATABASES;

-- Table Operations
CREATE TABLE table_name (...);
DROP TABLE table_name;
ALTER TABLE table_name ADD COLUMN col_name datatype;
ALTER TABLE table_name MODIFY COLUMN col_name new_datatype;
ALTER TABLE table_name DROP COLUMN col_name;
TRUNCATE TABLE table_name;
RENAME TABLE old_name TO new_name;

-- Data Operations
INSERT INTO table_name VALUES (...);
UPDATE table_name SET col = value WHERE condition;
DELETE FROM table_name WHERE condition;
SELECT * FROM table_name WHERE condition;

-- View Structure
DESC table_name;
SHOW TABLES;
SHOW COLUMNS FROM table_name;
```

---

## Best Practices

1. **Always use PRIMARY KEY** for unique identification
2. **Use FOREIGN KEY** to maintain referential integrity
3. **Add NOT NULL** constraints to required fields
4. **Use appropriate data types** to save storage
5. **Index frequently queried columns** for better performance
6. **Use transactions** for critical operations
7. **Normalize tables** to reduce redundancy
8. **Use JOINs instead of multiple queries** for better performance
9. **Avoid SELECT *** in production (specify columns)
10. **Use LIMIT** when testing queries on large tables

---

## Additional Resources

- [MySQL Official Documentation](https://dev.mysql.com/doc/)
- [MySQL Tutorial by W3Schools](https://www.w3schools.com/mysql/)
- [SQL Practice on LeetCode](https://leetcode.com/problemset/database/)

---

**Created for:** Database Management System Learning  
**Last Updated:** October 2025  
**License:** MIT

---

## Contributing

Feel free to fork this repository and add your own examples or improvements!

**Happy Learning! 🚀📊**