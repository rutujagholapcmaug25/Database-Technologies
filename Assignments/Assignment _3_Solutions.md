# SQL Employee Management System - Queries and Solutions

## Database Setup

```sql
CREATE DATABASE EMS;
USE EMS;
```

---

## 1. Create Employee & Department Tables with Specific Columns and Constraints

### Question
Write a SQL query to create Employee & Department table with specific columns and constraints.

### Command
```sql
CREATE TABLE DEPARTMENT(
    DEPTID INT PRIMARY KEY, 
    DEPTNAME VARCHAR(50) NOT NULL
);

CREATE TABLE EMPLOYEE(
    EMPID INT PRIMARY KEY, 
    EMPNAME VARCHAR(50) NOT NULL, 
    SALARY DECIMAL(10,2), 
    DEPTID INT, 
    JOININGDATE DATE, 
    CONSTRAINT FK_DEPT FOREIGN KEY(DEPTID) REFERENCES DEPARTMENT(DEPTID)
);
```

### Output
```
Query OK, 0 rows affected (0.10 sec)
Query OK, 0 rows affected (0.08 sec)
```

### Concepts Used
- **DDL (Data Definition Language)**: CREATE TABLE
- **Primary Key Constraint**: Ensures uniqueness
- **Foreign Key Constraint**: Maintains referential integrity between tables
- **NOT NULL Constraint**: Ensures mandatory fields
- **Data Types**: INT, VARCHAR, DECIMAL, DATE

---

## 2. Add a New Column to an Existing Table

### Question
Write a SQL query to add any new column to an existing table.

### Command
```sql
ALTER TABLE EMPLOYEE ADD EMAIL VARCHAR(50);
```

### Output
```
Query OK, 0 rows affected (0.05 sec)
```

### Verify Structure
```sql
DESC EMPLOYEE;
```

```
+-------------+---------------+------+-----+---------+-------+
| Field       | Type          | Null | Key | Default | Extra |
+-------------+---------------+------+-----+---------+-------+
| EMPID       | int           | NO   | PRI | NULL    |       |
| EMPNAME     | varchar(50)   | NO   |     | NULL    |       |
| SALARY      | decimal(10,2) | YES  |     | NULL    |       |
| DEPTID      | int           | YES  | MUL | NULL    |       |
| JOININGDATE | date          | YES  |     | NULL    |       |
| EMAIL       | varchar(50)   | YES  |     | NULL    |       |
+-------------+---------------+------+-----+---------+-------+
```

### Concepts Used
- **DDL**: ALTER TABLE
- **Schema Modification**: Adding columns to existing tables
- **DESC Command**: Describes table structure

---

## 3. Insert Multiple Records into a Table in a Single Operation

### Question
Write a SQL query to insert multiple records (At least 5) into a table in a single operation.

### Command
```sql
INSERT INTO DEPARTMENT(DEPTID, DEPTNAME) 
VALUES(1, 'HR'), (2, 'IT'), (3, 'FINANCE');

INSERT INTO EMPLOYEE (EMPID, EMPNAME, SALARY, DEPTID, JOININGDATE) 
VALUES
    (101, 'RUTUJA GHOLAP', 80000, 1, '20250201'),
    (102, 'ANUJA GHOLAP', 70000, 2, '20240809'),
    (103, 'PRERNA DIWATE', 50000, 1, '20250906'),
    (104, 'SANIKA SHINDE', 40000, 3, '20250101'),
    (106, 'SUMIT DHIVAR', 35000, 1, '20250901'),
    (107, 'DNYANESHWARI', 80000, 1, '20240808'),
    (108, 'KANCHAN SURYAVANSHI', 50000, 2, '20250904'),
    (109, 'VRUNDA BATWAL', 40000, 3, '20250102');
```

### Output
```
Query OK, 3 rows affected (0.02 sec)
Records: 3  Duplicates: 0  Warnings: 0

Query OK, 8 rows affected (0.01 sec)
Records: 8  Duplicates: 0  Warnings: 0
```

### Concepts Used
- **DML (Data Manipulation Language)**: INSERT
- **Bulk Insert**: Multiple VALUES in single INSERT statement
- **Performance Optimization**: Single operation instead of multiple

---

## 4. Delete All Records from a Table

### Question
Write a SQL query to delete all records from a table.

### Command
```sql
DELETE FROM EMPLOYEE;
```

### Output
```
Query OK, 9 rows affected (0.01 sec)
```

### Verify
```sql
SELECT * FROM EMPLOYEE;
```

```
Empty set (0.00 sec)
```

### Concepts Used
- **DML**: DELETE
- **Unconditional Delete**: Removes all rows
- **Note**: DELETE keeps table structure intact, unlike TRUNCATE

---

## 5. Update Salary Records Where ID = 5

### Question
Write a SQL query to update salary records from a table where id = 5.

### Command
```sql
UPDATE EMPLOYEE SET SALARY = SALARY + 5000 WHERE EMPID = 2;
```

### Output
```
Query OK, 0 rows affected (0.01 sec)
Rows matched: 0  Changed: 0  Warnings: 0
```

**Note**: No record with EMPID = 2 exists in the table, hence 0 rows affected.

### Concepts Used
- **DML**: UPDATE
- **WHERE Clause**: Conditional filtering
- **Arithmetic Operations**: SALARY + 5000

---

## 6. Grant SELECT Privilege on Table to User

### Question
Write a query to grant SELECT privilege on the Emp table to user readonly_user.

### Command
```sql
CREATE USER 'READONLY_USER'@'LOCALHOST' IDENTIFIED BY 'PASS@123';
GRANT SELECT ON EMPLOYEE TO 'READONLY_USER'@'LOCALHOST';
FLUSH PRIVILEGES;
```

### Output
```
Query OK, 0 rows affected (0.05 sec)
Query OK, 0 rows affected (0.01 sec)
Query OK, 0 rows affected (0.01 sec)
```

### Concepts Used
- **DCL (Data Control Language)**: GRANT
- **User Management**: CREATE USER
- **Access Control**: Granting specific privileges
- **FLUSH PRIVILEGES**: Reloads privilege tables

---

## 7. Revoke INSERT Privilege from User

### Question
Write a query to revoke INSERT privilege on the Emp table from user temp_user.

### Command
```sql
CREATE USER 'TEMP_USER'@'LOCALHOST' IDENTIFIED BY 'PASS@111';
GRANT SELECT ON EMPLOYEE TO 'TEMP_USER'@'LOCALHOST';
REVOKE INSERT ON EMPLOYEE FROM 'TEMP_USER'@'LOCALHOST';
```

### Output
```
Query OK, 0 rows affected (0.02 sec)
Query OK, 0 rows affected (0.01 sec)
Query OK, 0 rows affected (0.01 sec)
```

### Concepts Used
- **DCL**: REVOKE
- **Privilege Management**: Removing specific permissions
- **Security**: Controlling user access levels

---

## 8. Insert New Employee and Commit Transaction

### Question
Write a query to insert a new employee into the Emp table and commit the transaction.

### Command
```sql
INSERT INTO EMPLOYEE (EMPID, EMPNAME, SALARY, DEPTID, JOININGDATE) 
VALUES(110, 'SHRIKANT', 90000, 2, '20250803');

COMMIT;
```

### Output
```
Query OK, 1 row affected (0.01 sec)
Query OK, 0 rows affected (0.00 sec)
```

### Concepts Used
- **TCL (Transaction Control Language)**: COMMIT
- **Transaction Management**: Making changes permanent
- **ACID Properties**: Ensures data consistency

---

## 9. Update Salary and Commit Transaction

### Question
Write a query to update the Salary of an employee with EmpID = 101 and commit the transaction.

### Command
```sql
UPDATE EMPLOYEE SET SALARY = 100000 WHERE EMPID = 105;
COMMIT;
```

### Output
```
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

Query OK, 0 rows affected (0.00 sec)
```

### Concepts Used
- **DML**: UPDATE
- **TCL**: COMMIT
- **Transaction Persistence**: Saving changes permanently

---

## 10. Set SAVEPOINT Before Updating DeptID

### Question
Write a query to set a SAVEPOINT before updating the DeptID of an employee.

### Command
```sql
START TRANSACTION;
SAVEPOINT BEFORE_UPDATE;
UPDATE Employee SET DeptID = 2 WHERE EmpID = 107;
```

### Output
```
Query OK, 0 rows affected (0.00 sec)
Query OK, 0 rows affected (0.00 sec)
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

### Concepts Used
- **TCL**: SAVEPOINT
- **Partial Rollback**: Creating restore points within transactions
- **Transaction Control**: Fine-grained control over changes

---

## 11. Rollback to Previously Created SAVEPOINT

### Question
Write a query to rollback to the previously created SAVEPOINT.

### Command
```sql
ROLLBACK TO BEFORE_UPDATE;
COMMIT;
```

### Output
```
Query OK, 0 rows affected (0.01 sec)
Query OK, 0 rows affected (0.00 sec)
```

### Verify
```sql
SELECT * FROM EMPLOYEE WHERE EMPID = 107;
```

```
+-------+--------------+----------+--------+-------------+-------+
| EMPID | EMPNAME      | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+--------------+----------+--------+-------------+-------+
|   107 | DNYANESHWARI | 80000.00 |      3 | 2024-08-08  | NULL  |
+-------+--------------+----------+--------+-------------+-------+
```

**Note**: The update was rolled back, so DEPTID remains 3.

### Concepts Used
- **TCL**: ROLLBACK TO SAVEPOINT
- **Partial Transaction Rollback**: Undoing changes to a specific point
- **Data Recovery**: Reverting unwanted changes

---

## 12. Truncate All Data from Table

### Question
Write a query to truncate all data from the Emp table.

### Command
```sql
TRUNCATE TABLE EMPLOYEE;
```

### Output
```
Query OK, 0 rows affected (0.06 sec)
```

### Verify
```sql
SELECT * FROM EMPLOYEE;
```

```
Empty set (0.00 sec)
```

### Concepts Used
- **DDL**: TRUNCATE
- **Fast Delete**: Removes all rows without logging individual deletions
- **Cannot Rollback**: DDL operations are auto-committed
- **Reset Auto-increment**: Resets identity columns

---

## 13. List Employees with Salary Greater Than 50,000

### Question
Write a query to list all employees whose salary is greater than 50,000.

### Command
```sql
SELECT * FROM EMPLOYEE WHERE SALARY > 50000;
```

### Output
```
+-------+---------------+----------+--------+-------------+-------+
| EMPID | EMPNAME       | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+---------------+----------+--------+-------------+-------+
|   101 | RUTUJA GHOLAP | 80000.00 |      1 | 2025-02-01  | NULL  |
|   102 | ANUJA GHOLAP  | 70000.00 |      2 | 2024-08-09  | NULL  |
|   107 | DNYANESHWARI  | 80000.00 |      1 | 2024-08-08  | NULL  |
+-------+---------------+----------+--------+-------------+-------+
```

### Concepts Used
- **DQL (Data Query Language)**: SELECT
- **WHERE Clause**: Filtering with conditions
- **Comparison Operators**: Greater than (>)

---

## 14. Increase Salary by 10% for Specific Department

### Question
Write a query to increase the salary of all employees in DeptID = 10 by 10%.

### Command
```sql
UPDATE EMPLOYEE SET SALARY = SALARY * 1.10 WHERE DEPTID = 1;
```

### Output
```
Query OK, 4 rows affected (0.13 sec)
Rows matched: 4  Changed: 4  Warnings: 0
```

### Concepts Used
- **DML**: UPDATE
- **Arithmetic Operations**: Percentage calculation
- **Conditional Update**: WHERE clause filtering
- **Bulk Update**: Affects multiple rows

---

## 15. Count Total Employees in Each Department

### Question
Write a query to count the total number of employees in each department.

### Command
```sql
SELECT DEPTID, COUNT(*) AS TOTALEMPLOYEES 
FROM EMPLOYEE 
GROUP BY DEPTID;
```

### Output
```
+--------+----------------+
| DEPTID | TOTALEMPLOYEES |
+--------+----------------+
|      1 |              4 |
|      2 |              2 |
|      3 |              2 |
+--------+----------------+
```

### Concepts Used
- **Aggregate Functions**: COUNT(*)
- **GROUP BY Clause**: Grouping data by department
- **Column Alias**: AS TOTALEMPLOYEES
- **Data Analysis**: Statistical aggregation

---

## 16. List Departments with No Employees

### Question
Write a query to list departments that have no employees 

### Command
```sql
SELECT D.DEPTID, D.DEPTNAME 
FROM DEPARTMENT D 
LEFT JOIN EMPLOYEE E ON D.DEPTID = E.DEPTID 
WHERE E.EMPID IS NULL;
```

### Concepts Used
- **JOIN Operations**: LEFT JOIN
- **NULL Checking**: IS NULL
- **Set Operations**: Finding non-matching records
- **Views**: Can be created for specific filtered data


---

## 17. Display Highest Salary Among All Employees

### Question
Write a query to display the highest salary among all employees.

### Command
```sql
SELECT MAX(SALARY) AS HIGH_SALARYL FROM EMPLOYEE;
```

### Output
```
+--------------+
| HIGH_SALARYL |
+--------------+
|     88000.00 |
+--------------+
```

### Concepts Used
- **Aggregate Functions**: MAX()
- **Column Alias**: AS HIGH_SALARYL
- **Statistical Analysis**: Finding maximum values

---

## 18. Display 2nd Highest Salary

### Question
Write a query to display the 2nd highest salary among all employees.

### Command
```sql
SELECT MAX(SALARY) AS SECOND_HIGHEST_SALARYL 
FROM EMPLOYEE 
WHERE SALARY < (SELECT MAX(SALARY) FROM EMPLOYEE);
```

### Output
```
+------------------------+
| SECOND_HIGHEST_SALARYL |
+------------------------+
|               70000.00 |
+------------------------+
```

### Concepts Used
- **Subqueries**: Nested SELECT statements
- **Aggregate Functions**: MAX()
- **Comparison Operators**: Less than (<)
- **Complex Queries**: Multi-level filtering

---

## 19. Show Employees Ordered by JoiningDate (Descending)

### Question
Write a query to show all employees ordered by JoiningDate in descending order.

### Command
```sql
SELECT * FROM EMPLOYEE ORDER BY JOININGDATE DESC;
```

### Output
```
+-------+---------------------+----------+--------+-------------+-------+
| EMPID | EMPNAME             | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+---------------------+----------+--------+-------------+-------+
|   103 | PRERNA DIWATE       | 55000.00 |      1 | 2025-09-06  | NULL  |
|   108 | KANCHAN SURYAVANSHI | 50000.00 |      2 | 2025-09-04  | NULL  |
|   106 | SUMIT DHIVAR        | 38500.00 |      1 | 2025-09-01  | NULL  |
|   101 | RUTUJA GHOLAP       | 88000.00 |      1 | 2025-02-01  | NULL  |
|   109 | VRUNDA BATWAL       | 40000.00 |      3 | 2025-01-02  | NULL  |
|   104 | SANIKA SHINDE       | 40000.00 |      3 | 2025-01-01  | NULL  |
|   102 | ANUJA GHOLAP        | 70000.00 |      2 | 2024-08-09  | NULL  |
|   107 | DNYANESHWARI        | 88000.00 |      1 | 2024-08-08  | NULL  |
+-------+---------------------+----------+--------+-------------+-------+
```

### Concepts Used
- **ORDER BY Clause**: Sorting results
- **DESC Keyword**: Descending order
- **Date Sorting**: Ordering by date fields

---

## 20. Rename Column DeptName to DepartmentName

### Question
Write a query to rename the DeptName column to DepartmentName.

### Command
```sql
ALTER TABLE DEPARTMENT RENAME COLUMN DEPTNAME TO DEPT_NAME;
```

### Output
```
Query OK, 0 rows affected (0.11 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

### Verify
```sql
DESC DEPARTMENT;
```

```
+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| DEPTID    | int         | NO   | PRI | NULL    |       |
| DEPT_NAME | varchar(50) | NO   |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
```

### Concepts Used
- **DDL**: ALTER TABLE
- **Schema Modification**: RENAME COLUMN
- **Metadata Changes**: Changing column names without affecting data

---

## Summary of SQL Categories Used

1. **DDL (Data Definition Language)**: CREATE, ALTER, TRUNCATE
2. **DML (Data Manipulation Language)**: INSERT, UPDATE, DELETE, SELECT
3. **DCL (Data Control Language)**: GRANT, REVOKE
4. **TCL (Transaction Control Language)**: COMMIT, ROLLBACK, SAVEPOINT
5. **DQL (Data Query Language)**: SELECT with various clauses

## Key Learning Points

- Proper table design with constraints ensures data integrity
- Transaction control provides data consistency and recovery mechanisms
- User privilege management enhances database security
- Aggregate functions and grouping enable powerful data analysis
- JOIN operations allow combining data from multiple tables
- Subqueries enable complex filtering and calculations
