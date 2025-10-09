# Employee Management System - SQL Queries Guide

## Database Schema

### Table Structures

#### 1. Department Table
```sql
CREATE TABLE department (
    DeptID INT PRIMARY KEY AUTO_INCREMENT,
    DeptName VARCHAR(50),
    Location VARCHAR(50)
);
```

#### 2. Employee Table
```sql
CREATE TABLE employee (
    EmpID INT PRIMARY KEY AUTO_INCREMENT,
    EmpName VARCHAR(50),
    DeptID INT,
    Salary DECIMAL(10,2),
    Designation VARCHAR(50),
    FOREIGN KEY (DeptID) REFERENCES department(DeptID)
);
```

#### 3. Project Table
```sql
CREATE TABLE project (
    ProjectID INT PRIMARY KEY AUTO_INCREMENT,
    ProjectName VARCHAR(50),
    DeptID INT,
    Budget DECIMAL(10,2)
);
```

---

## Sample Data

### Department Table Data
```sql
INSERT INTO department (DeptName, Location) VALUES
('HR', 'Mumbai'),
('Finance', 'Pune'),
('IT', 'Bangalore'),
('Marketing', 'Delhi'),
('Operations', 'Mumbai');
```

**Output:**
```
+--------+------------+-----------+
| DeptID | DeptName   | Location  |
+--------+------------+-----------+
|      1 | HR         | Mumbai    |
|      2 | Finance    | Pune      |
|      3 | IT         | Bangalore |
|      4 | Marketing  | Delhi     |
|      5 | Operations | Mumbai    |
+--------+------------+-----------+
```

### Employee Table Data
```sql
INSERT INTO employee (EmpName, DeptID, Salary, Designation) VALUES
('Amit', 1, 55000.00, 'Manager'),
('Riya', 2, 48000.00, 'Accountant'),
('Karan', 3, 62000.00, 'Developer'),
('Neha', NULL, 45000.00, 'Intern'),
('Suresh', 5, 53000.00, 'Supervisor');
```

**Output:**
```
+-------+---------+--------+----------+-------------+
| EmpID | EmpName | DeptID | Salary   | Designation |
+-------+---------+--------+----------+-------------+
|     1 | Amit    |      1 | 55000.00 | Manager     |
|     2 | Riya    |      2 | 48000.00 | Accountant  |
|     3 | Karan   |      3 | 62000.00 | Developer   |
|     4 | Neha    |   NULL | 45000.00 | Intern      |
|     5 | Suresh  |      5 | 53000.00 | Supervisor  |
+-------+---------+--------+----------+-------------+
```

### Project Table Data
```sql
INSERT INTO project (ProjectName, DeptID, Budget) VALUES
('Recruitment Portal', 1, 200000.00),
('Accounting System', 2, 300000.00),
('E-Commerce Site', 3, 500000.00),
('Ad Campaign', 4, 150000.00),
('Logistics App', 5, 250000.00);
```

**Output:**
```
+-----------+--------------------+--------+-----------+
| ProjectID | ProjectName        | DeptID | Budget    |
+-----------+--------------------+--------+-----------+
|         1 | Recruitment Portal |      1 | 200000.00 |
|         2 | Accounting System  |      2 | 300000.00 |
|         3 | E-Commerce Site    |      3 | 500000.00 |
|         4 | Ad Campaign        |      4 | 150000.00 |
|         5 | Logistics App      |      5 | 250000.00 |
+-----------+--------------------+--------+-----------+
```

---

## Query Solutions

### 1. Cartesian Product (Cross Join) - Relational Algebra

**Concept:** Cartesian Product combines every row from the first table with every row from the second table.

**Relational Algebra Expression:**
```
Employee × Department
```

**Explanation:** The symbol × (sigma) represents the Cartesian product operation. This combines all tuples from Employee with all tuples from Department, resulting in m × n rows (where m = rows in Employee, n = rows in Department).

---

### 2. INNER JOIN between Employee and Department

**Concept:** INNER JOIN returns only matching records from both tables based on a condition.

**Approach:** Match rows where the DeptID in both tables is equal.

**SQL Query:**
```sql
SELECT e.EmpName, d.DeptName
FROM employee e
INNER JOIN department d ON e.DeptID = d.DeptID;
```

**Output:**
```
+---------+------------+
| empName | deptName   |
+---------+------------+
| Amit    | HR         |
| Riya    | Finance    |
| Karan   | IT         |
| Suresh  | Operations |
+---------+------------+
```

**Explanation:** Only 4 employees are shown because Neha has NULL DeptID and doesn't match any department.

---

### 3. LEFT OUTER JOIN

**Concept:** LEFT JOIN returns all records from the left table and matching records from the right table. Non-matching rows show NULL for right table columns.

**Approach:** Display all employees regardless of whether they have a department assigned.

**SQL Query:**
```sql
SELECT e.EmpName, e.Designation, d.DeptName, d.Location
FROM employee e
LEFT JOIN department d ON e.DeptID = d.DeptID;
```

**Output:**
```
+---------+-------------+------------+-----------+
| empName | designation | deptName   | location  |
+---------+-------------+------------+-----------+
| Amit    | Manager     | HR         | Mumbai    |
| Riya    | Accountant  | Finance    | Pune      |
| Karan   | Developer   | IT         | Bangalore |
| Neha    | Intern      | NULL       | NULL      |
| Suresh  | Supervisor  | Operations | Mumbai    |
+---------+-------------+------------+-----------+
```

**Explanation:** All 5 employees appear. Neha shows NULL for department fields since she has no department assigned.

---

### 4. RIGHT OUTER JOIN

**Concept:** RIGHT JOIN returns all records from the right table and matching records from the left table. Non-matching rows show NULL for left table columns.

**Approach:** Display all departments, even those without employees.

**SQL Query:**
```sql
SELECT e.EmpName, e.Designation, d.DeptName, d.Location
FROM employee e
RIGHT JOIN department d ON e.DeptID = d.DeptID;
```

**Output:**
```
+---------+-------------+------------+-----------+
| empName | designation | deptName   | location  |
+---------+-------------+------------+-----------+
| Amit    | Manager     | HR         | Mumbai    |
| Riya    | Accountant  | Finance    | Pune      |
| Karan   | Developer   | IT         | Bangalore |
| NULL    | NULL        | Marketing  | Delhi     |
| Suresh  | Supervisor  | Operations | Mumbai    |
+---------+-------------+------------+-----------+
```

**Explanation:** All 5 departments appear. Marketing shows NULL for employee fields since no employee works in that department.

---

### 5. NATURAL JOIN

**Concept:** NATURAL JOIN automatically joins tables based on columns with the same name in both tables.

**Approach:** MySQL automatically matches on DeptID (common column name).

**SQL Query:**
```sql
SELECT * FROM employee NATURAL JOIN department;
```

**Output:**
```
+--------+-------+---------+----------+-------------+------------+-----------+
| DeptID | EmpID | EmpName | Salary   | Designation | DeptName   | Location  |
+--------+-------+---------+----------+-------------+------------+-----------+
|      1 |     1 | Amit    | 55000.00 | Manager     | HR         | Mumbai    |
|      2 |     2 | Riya    | 48000.00 | Accountant  | Finance    | Pune      |
|      3 |     3 | Karan   | 62000.00 | Developer   | IT         | Bangalore |
|      5 |     5 | Suresh  | 53000.00 | Supervisor  | Operations | Mumbai    |
+--------+-------+---------+----------+-------------+------------+-----------+
```

**Explanation:** Only employees with matching DeptID appear. The common column (DeptID) appears only once in the result.

---

### 6. CROSS JOIN between Employee and Project

**Concept:** CROSS JOIN produces a Cartesian product - every employee paired with every project.

**Approach:** Combine all rows from Employee with all rows from Project (5 × 5 = 25 rows).

**SQL Query:**
```sql
SELECT e.EmpName, p.ProjectName 
FROM employee e
CROSS JOIN project p;
```

**Output:**
```
+---------+--------------------+
| empName | projectName        |
+---------+--------------------+
| Suresh  | Recruitment Portal |
| Neha    | Recruitment Portal |
| Karan   | Recruitment Portal |
| Riya    | Recruitment Portal |
| Amit    | Recruitment Portal |
| Suresh  | Accounting System  |
| Neha    | Accounting System  |
| Karan   | Accounting System  |
| Riya    | Accounting System  |
| Amit    | Accounting System  |
| Suresh  | E-Commerce Site    |
| Neha    | E-Commerce Site    |
| Karan   | E-Commerce Site    |
| Riya    | E-Commerce Site    |
| Amit    | E-Commerce Site    |
| Suresh  | Ad Campaign        |
| Neha    | Ad Campaign        |
| Karan   | Ad Campaign        |
| Riya    | Ad Campaign        |
| Amit    | Ad Campaign        |
| Suresh  | Logistics App      |
| Neha    | Logistics App      |
| Karan   | Logistics App      |
| Riya    | Logistics App      |
| Amit    | Logistics App      |
+---------+--------------------+
25 rows in set
```

**Explanation:** Each of 5 employees is paired with each of 5 projects, resulting in 25 combinations.

---

### 7. Create EmpBackup Table (Structure Only)

**Concept:** Create a table with the same structure but no data using WHERE condition that's always false.

**Approach:** Use `WHERE 1=0` to ensure no rows are copied, only the table structure.

**SQL Query:**
```sql
CREATE TABLE empBackup AS 
SELECT * FROM employee WHERE 1 = 0;
```

**Verification:**
```sql
DESC empBackup;
```

**Output:**
```
+-------------+---------------+------+-----+---------+-------+
| Field       | Type          | Null | Key | Default | Extra |
+-------------+---------------+------+-----+---------+-------+
| EmpID       | int           | NO   |     | NULL    |       |
| EmpName     | varchar(50)   | YES  |     | NULL    |       |
| DeptID      | int           | YES  |     | NULL    |       |
| Salary      | decimal(10,2) | YES  |     | NULL    |       |
| Designation | varchar(50)   | YES  |     | NULL    |       |
+-------------+---------------+------+-----+---------+-------+
```

**Explanation:** The table has the same columns and data types as employee, but contains no data.

---

### 8. Copy All Data from Employee to EmpBackup

**Concept:** Insert all rows from one table into another with identical structure.

**Approach:** Use INSERT INTO ... SELECT statement.

**SQL Query:**
```sql
INSERT INTO empBackup SELECT * FROM employee;
```

**Verification:**
```sql
SELECT * FROM empBackup;
```

**Output:**
```
+-------+---------+--------+----------+-------------+
| EmpID | EmpName | DeptID | Salary   | Designation |
+-------+---------+--------+----------+-------------+
|     1 | Amit    |      1 | 55000.00 | Manager     |
|     2 | Riya    |      2 | 48000.00 | Accountant  |
|     3 | Karan   |      3 | 62000.00 | Developer   |
|     4 | Neha    |   NULL | 45000.00 | Intern      |
|     5 | Suresh  |      5 | 53000.00 | Supervisor  |
+-------+---------+--------+----------+-------------+
```

**Explanation:** All 5 employee records are copied to empBackup table.

---

### 9. Create ProjectArchive (Structure + Data)

**Concept:** Create a new table with both structure and data from an existing table.

**Approach:** Use CREATE TABLE AS SELECT without any WHERE clause.

**SQL Query:**
```sql
CREATE TABLE projectArchive AS 
SELECT * FROM project;
```

**Verification:**
```sql
SELECT * FROM projectArchive;
```

**Output:**
```
+-----------+--------------------+--------+-----------+
| ProjectID | ProjectName        | DeptID | Budget    |
+-----------+--------------------+--------+-----------+
|         1 | Recruitment Portal |      1 | 200000.00 |
|         2 | Accounting System  |      2 | 300000.00 |
|         3 | E-Commerce Site    |      3 | 500000.00 |
|         4 | Ad Campaign        |      4 | 150000.00 |
|         5 | Logistics App      |      5 | 250000.00 |
+-----------+--------------------+--------+-----------+
```

**Explanation:** Both the table structure and all 5 project records are copied to projectArchive.

---

### 10. Create Table with AUTO_INCREMENT

**Concept:** AUTO_INCREMENT automatically generates unique sequential values for a column.

**Approach:** Define EmpID as PRIMARY KEY with AUTO_INCREMENT during table creation.

**SQL Query:**
```sql
CREATE TABLE employee (
    EmpID INT PRIMARY KEY AUTO_INCREMENT,
    EmpName VARCHAR(50),
    DeptID INT,
    Salary DECIMAL(10,2),
    Designation VARCHAR(50),
    FOREIGN KEY (DeptID) REFERENCES department(DeptID)
);
```

**Explanation:** 
- `AUTO_INCREMENT` starts at 1 by default
- Each new INSERT automatically gets the next sequential number
- No need to manually specify EmpID when inserting data

**Example Insert:**
```sql
INSERT INTO employee (EmpName, DeptID, Salary, Designation) 
VALUES ('Amit', 1, 55000.00, 'Manager');
-- EmpID will automatically be 1
```

---

### 11. Employees with Salary > Average Salary (Subquery)

**Concept:** Use a subquery to calculate average salary and compare each employee's salary against it.

**Approach:** 
1. Inner query calculates AVG(Salary)
2. Outer query filters employees where Salary > average

**SQL Query:**
```sql
SELECT EmpName, Salary
FROM employee
WHERE Salary > (SELECT AVG(Salary) FROM employee);
```

**Output:**
```
+---------+----------+
| EmpName | Salary   |
+---------+----------+
| Amit    | 55000.00 |
| Karan   | 62000.00 |
| Suresh  | 53000.00 |
+---------+----------+
```

**Explanation:** 
- Average salary = (55000 + 48000 + 62000 + 45000 + 53000) / 5 = 52,600
- Three employees earn more than 52,600

---

### 12. Employees in Mumbai Departments (Subquery in WHERE)

**Concept:** Use a subquery to find DeptIDs of Mumbai departments, then find employees in those departments.

**Approach:**
1. Inner query finds DeptIDs where Location = 'Mumbai'
2. Outer query finds employees with those DeptIDs using IN operator

**SQL Query:**
```sql
SELECT EmpName
FROM employee
WHERE DeptID IN (
    SELECT DeptID FROM department WHERE Location = 'Mumbai'
);
```

**Output:**
```
+---------+
| EmpName |
+---------+
| Amit    |
| Suresh  |
+---------+
```

**Explanation:** 
- Mumbai departments: HR (DeptID=1) and Operations (DeptID=5)
- Amit works in HR, Suresh works in Operations

---

### 13. Departments with More Than 5 Employees

**Concept:** Use GROUP BY with HAVING to count employees per department, then filter using a subquery.

**Approach:**
1. Inner query groups employees by DeptID and counts them
2. HAVING filters groups with count > 5
3. Outer query shows department names

**SQL Query:**
```sql
SELECT DeptName
FROM department
WHERE DeptID IN (
    SELECT DeptID
    FROM employee
    GROUP BY DeptID
    HAVING COUNT(EmpID) > 5
);
```

**Output:**
```
Empty set
```

**Explanation:** No department has more than 5 employees in our sample data. Each department has at most 1 employee.

**Modified Query (> 0 employees):**
```sql
SELECT DeptName
FROM department
WHERE DeptID IN (
    SELECT DeptID
    FROM employee
    WHERE DeptID IS NOT NULL
    GROUP BY DeptID
    HAVING COUNT(EmpID) > 0
);
```

---

### 14. Find Maximum Salary Using Subquery

**Concept:** Use MAX() aggregate function in a subquery to find the highest salary and identify employee(s) with that salary.

**Approach:**
1. Inner query finds MAX(Salary)
2. Outer query filters employees with that exact salary

**SQL Query:**
```sql
SELECT EmpName, Salary
FROM employee
WHERE Salary = (SELECT MAX(Salary) FROM employee);
```

**Output:**
```
+---------+----------+
| EmpName | Salary   |
+---------+----------+
| Karan   | 62000.00 |
+---------+----------+
```

**Explanation:** Karan has the highest salary of ₹62,000 among all employees.

---

### 15. Self Join Queries

#### A. Self Join on Employee Table

**Concept:** Self join compares rows within the same table by treating it as two separate tables.

**Use Case:** Find pairs of employees where one earns less than the other.

**SQL Query:**
```sql
SELECT e1.EmpName AS Employee, e2.EmpName AS HigherPaidEmployee
FROM employee e1
JOIN employee e2 ON e1.Salary < e2.Salary;
```

**Output:**
```
+----------+--------------------+
| Employee | HigherPaidEmployee |
+----------+--------------------+
| Suresh   | Amit               |
| Neha     | Amit               |
| Riya     | Amit               |
| Neha     | Riya               |
| Suresh   | Karan              |
| Neha     | Karan              |
| Riya     | Karan              |
| Amit     | Karan              |
| Neha     | Suresh             |
| Riya     | Suresh             |
+----------+--------------------+
```

**Explanation:** Each employee is compared with every other employee who earns more, showing salary hierarchy relationships.

---

#### B. Self Join on Department Table

**Concept:** Find departments in the same location.

**Use Case:** Identify departments that could share resources due to same location.

**SQL Query:**
```sql
SELECT d1.DeptName AS Dept1, d2.DeptName AS SameCityDept, d1.Location
FROM department d1
JOIN department d2
ON d1.Location = d2.Location AND d1.DeptID <> d2.DeptID;
```

**Output:**
```
+------------+--------------+----------+
| Dept1      | SameCityDept | Location |
+------------+--------------+----------+
| Operations | HR           | Mumbai   |
| HR         | Operations   | Mumbai   |
+------------+--------------+----------+
```

**Explanation:** HR and Operations are both in Mumbai, so they appear as pairs. The condition `d1.DeptID <> d2.DeptID` prevents a department from being paired with itself.

---

#### C. Self Join on Project Table

**Concept:** Find projects in the same department.

**Use Case:** Identify multiple projects managed by the same department.

**SQL Query:**
```sql
SELECT p1.ProjectName AS Project1, p2.ProjectName AS SameDeptProject
FROM project p1
JOIN project p2
ON p1.DeptID = p2.DeptID AND p1.ProjectID <> p2.ProjectID;
```

**Output:**
```
Empty set
```

**Explanation:** Each department has only one project, so no pairs exist. If multiple projects existed per department, they would be shown here.

---

## Key Concepts Summary

### Join Types Comparison

| Join Type | Returns | Use Case |
|-----------|---------|----------|
| **INNER JOIN** | Only matching rows | When you need records that exist in both tables |
| **LEFT JOIN** | All left + matching right | When you need all records from left table |
| **RIGHT JOIN** | All right + matching left | When you need all records from right table |
| **CROSS JOIN** | Cartesian product | When you need all possible combinations |
| **NATURAL JOIN** | Auto-match on same column names | Quick joins with same column names |
| **SELF JOIN** | Compare rows in same table | Hierarchical or comparative analysis |

### Subquery Types

1. **Scalar Subquery**: Returns single value (e.g., AVG, MAX)
2. **Row Subquery**: Returns single row
3. **Table Subquery**: Returns multiple rows (use with IN, EXISTS)

### Important Notes

- **NULL handling**: Employees with NULL DeptID won't appear in INNER/NATURAL joins
- **AUTO_INCREMENT**: Automatically generates unique sequential IDs
- **Self joins**: Require aliases (e.g., e1, e2) to distinguish table instances
- **Subqueries**: Can be used in SELECT, WHERE, HAVING, and FROM clauses

---

## Complete Database Setup Script

```sql
-- Database Creation
CREATE DATABASE emp;
USE emp;

-- Table Creation
CREATE TABLE department (
    DeptID INT PRIMARY KEY AUTO_INCREMENT,
    DeptName VARCHAR(50),
    Location VARCHAR(50)
);

CREATE TABLE employee (
    EmpID INT PRIMARY KEY AUTO_INCREMENT,
    EmpName VARCHAR(50),
    DeptID INT,
    Salary DECIMAL(10,2),
    Designation VARCHAR(50),
    FOREIGN KEY (DeptID) REFERENCES department(DeptID)
);

CREATE TABLE project (
    ProjectID INT PRIMARY KEY AUTO_INCREMENT,
    ProjectName VARCHAR(50),
    DeptID INT,
    Budget DECIMAL(10,2)
);

-- Insert Sample Data
INSERT INTO department (DeptName, Location) VALUES
('HR', 'Mumbai'),
('Finance', 'Pune'),
('IT', 'Bangalore'),
('Marketing', 'Delhi'),
('Operations', 'Mumbai');

INSERT INTO employee (EmpName, DeptID, Salary, Designation) VALUES
('Amit', 1, 55000.00, 'Manager'),
('Riya', 2, 48000.00, 'Accountant'),
('Karan', 3, 62000.00, 'Developer'),
('Neha', NULL, 45000.00, 'Intern'),
('Suresh', 5, 53000.00, 'Supervisor');

INSERT INTO project (ProjectName, DeptID, Budget) VALUES
('Recruitment Portal', 1, 200000.00),
('Accounting System', 2, 300000.00),
('E-Commerce Site', 3, 500000.00),
('Ad Campaign', 4, 150000.00),
('Logistics App', 5, 250000.00);
```

---

## Practice Exercises

1. Write a query to find employees earning between 45000 and 55000
2. List all departments with their total project budgets
3. Find employees who work in the same department as 'Amit'
4. Calculate the average salary per location
5. Find departments with no employees assigned

---

## Additional Resources

- [MySQL Official Documentation](https://dev.mysql.com/doc/)
- [SQL Join Visualization](https://sql-joins.leopard.in.ua/)
- [Database Normalization Guide](https://www.guru99.com/database-normalization.html)

---

**Created for:** Database Management System Learning  
**Last Updated:** October 2025  
**License:** MIT

---

## Contributing

Feel free to contribute by:
- Adding more complex queries
- Providing optimization tips
- Sharing real-world use cases
- Reporting issues or corrections

**Happy Learning! 🚀**