# MySQL Department & Employee Database - Complete Guide

## Database Setup

### Create Database and Tables

```sql
CREATE DATABASE DEPT_EMP;
USE DEPT_EMP;

CREATE TABLE Dept (
    DeptID INT PRIMARY KEY,
    DeptName VARCHAR(50)
);

CREATE TABLE Emp (
    EmpID INT PRIMARY KEY,
    Name VARCHAR(50),
    Salary INT,
    DeptID INT,
    FOREIGN KEY (DeptID) REFERENCES Dept(DeptID)
);
```

### Insert Sample Data

```sql
INSERT INTO Dept VALUES
(1, 'HR'),
(2, 'IT'),
(3, 'Finance');

INSERT INTO Emp VALUES
(101, 'Alice',   30000, 1),
(102, 'Bob',     45000, 2),
(103, 'Charlie', 40000, 1),
(104, 'David',   55000, 3),
(105, 'Eva',     60000, 2),
(106, 'Frank',   50000, 3),
(107, 'Grace',   35000, 1),
(108, 'Hank',    70000, 2),
(109, 'Ivy',     45000, 3),
(110, 'Jack',    40000, 2);
```

---

## GROUP BY & HAVING Clause

### Concept
- **GROUP BY**: Groups rows that have the same values in specified columns
- **HAVING**: Filters grouped results (used after aggregation)
- **WHERE vs HAVING**: WHERE filters before grouping, HAVING filters after grouping

---

### Q1. List all employees with their department names

**Query:**
```sql
SELECT empID, name, salary, deptName 
FROM emp 
JOIN dept ON emp.deptID = dept.deptID;
```

**Output:**
```
+-------+---------+--------+----------+
| empID | name    | salary | deptName |
+-------+---------+--------+----------+
|   101 | Alice   |  30000 | HR       |
|   103 | Charlie |  40000 | HR       |
|   107 | Grace   |  35000 | HR       |
|   102 | Bob     |  45000 | IT       |
|   105 | Eva     |  60000 | IT       |
|   108 | Hank    |  70000 | IT       |
|   110 | Jack    |  40000 | IT       |
|   104 | David   |  55000 | Finance  |
|   106 | Frank   |  50000 | Finance  |
|   109 | Ivy     |  45000 | Finance  |
+-------+---------+--------+----------+
```

---

### Q2. Find the total salary of each department

**Query:**
```sql
SELECT deptname, SUM(salary) AS totalSalary
FROM emp
JOIN dept ON emp.deptID = dept.deptID
GROUP BY deptName;
```

**Output:**
```
+----------+-------------+
| deptname | totalSalary |
+----------+-------------+
| HR       |      105000 |
| IT       |      215000 |
| Finance  |      150000 |
+----------+-------------+
```

---

### Q3. Find the average salary of each department where average salary > 45000

**Query:**
```sql
SELECT deptName, AVG(salary) AS Avg_salary
FROM emp
JOIN dept ON emp.deptId = dept.deptID
GROUP BY deptName
HAVING AVG(Salary) > 45000;
```

**Output:**
```
+----------+------------+
| deptName | Avg_salary |
+----------+------------+
| IT       | 53750.0000 |
| Finance  | 50000.0000 |
+----------+------------+
```

---

### Q4. Find departments with more than 2 employees

**Query:**
```sql
SELECT deptName, COUNT(*) AS NumEmployees
FROM emp
JOIN dept ON emp.deptID = dept.deptID
GROUP BY deptname
HAVING COUNT(*) > 2;
```

**Output:**
```
+----------+--------------+
| deptName | NumEmployees |
+----------+--------------+
| HR       |            3 |
| IT       |            4 |
| Finance  |            3 |
+----------+--------------+
```

---

### Q5. Find the highest salary in each department

**Query:**
```sql
SELECT deptName, MAX(salary) AS Max_salary
FROM emp
JOIN dept ON emp.deptid = dept.deptid
GROUP BY deptname;
```

**Output:**
```
+----------+------------+
| deptName | Max_salary |
+----------+------------+
| HR       |      40000 |
| IT       |      70000 |
| Finance  |      55000 |
+----------+------------+
```

---

### Q6. Show departments where total salary > 150000, along with number of employees

**Query:**
```sql
SELECT deptname, SUM(salary) AS total_salary, COUNT(*) AS numOfEmp
FROM emp
JOIN dept ON emp.deptid = dept.deptid
GROUP BY deptname
HAVING SUM(salary) > 150000;
```

**Output:**
```
+----------+--------------+----------+
| deptname | total_salary | numOfEmp |
+----------+--------------+----------+
| IT       |       215000 |        4 |
+----------+--------------+----------+
```

---

### Q7. List employees who earn more than the average salary of their department

**Query:**
```sql
SELECT name, salary, deptname
FROM emp
JOIN dept ON emp.deptid = dept.deptid
WHERE salary > (
    SELECT AVG(salary) FROM emp WHERE deptid = emp.deptid
);
```

**Output:**
```
+-------+--------+----------+
| name  | salary | deptname |
+-------+--------+----------+
| David |  55000 | Finance  |
| Eva   |  60000 | IT       |
| Frank |  50000 | Finance  |
| Hank  |  70000 | IT       |
+-------+--------+----------+
```

---

## VIEWS

### Concept
- **VIEW**: A virtual table based on a SQL query
- Views don't store data physically; they store the query
- Useful for simplifying complex queries and providing security

### Create a View with JOIN

**Query:**
```sql
CREATE VIEW empDeptview AS 
SELECT empid, name, salary, deptname
FROM emp
JOIN dept ON emp.deptid = dept.deptid;
```

**Query the View:**
```sql
SELECT * FROM empdeptview;
```

**Output:**
```
+-------+---------+--------+----------+
| empid | name    | salary | deptname |
+-------+---------+--------+----------+
|   101 | Alice   |  30000 | HR       |
|   103 | Charlie |  40000 | HR       |
|   107 | Grace   |  35000 | HR       |
|   102 | Bob     |  45000 | IT       |
|   105 | Eva     |  60000 | IT       |
|   108 | Hank    |  70000 | IT       |
|   110 | Jack    |  40000 | IT       |
|   104 | David   |  55000 | Finance  |
|   106 | Frank   |  50000 | Finance  |
|   109 | Ivy     |  45000 | Finance  |
+-------+---------+--------+----------+
```

---

## SUBQUERIES

### Concept
- **Subquery**: A query nested inside another query
- **Types**:
  - **Scalar Subquery**: Returns single value
  - **Row Subquery**: Returns single row
  - **Table Subquery**: Returns multiple rows/columns
  - **Correlated Subquery**: References outer query columns

---

### Q1. Show each employee with their department name (scalar subquery)

**Query:**
```sql
SELECT name, salary, 
       (SELECT deptname FROM dept WHERE dept.deptid = emp.deptid) AS deptName 
FROM emp;
```

**Output:**
```
+---------+--------+----------+
| name    | salary | deptName |
+---------+--------+----------+
| Alice   |  30000 | HR       |
| Bob     |  45000 | IT       |
| Charlie |  40000 | HR       |
| David   |  55000 | Finance  |
| Eva     |  60000 | IT       |
| Frank   |  50000 | Finance  |
| Grace   |  35000 | HR       |
| Hank    |  70000 | IT       |
| Ivy     |  45000 | Finance  |
| Jack    |  40000 | IT       |
+---------+--------+----------+
```

---

### Q2. Show Employees earning more than their department average (correlated)

**Query:**
```sql
SELECT name, salary, deptname
FROM emp
JOIN dept ON emp.deptid = dept.deptid
WHERE salary > (
    SELECT AVG(salary) FROM emp WHERE deptid = emp.deptid
);
```

**Output:**
```
+-------+--------+----------+
| name  | salary | deptname |
+-------+--------+----------+
| David |  55000 | Finance  |
| Eva   |  60000 | IT       |
| Frank |  50000 | Finance  |
| Hank  |  70000 | IT       |
+-------+--------+----------+
```

---

### Q3. Show Employees whose salary equals the maximum in their department

**Query:**
```sql
SELECT name, salary, deptname
FROM emp
JOIN dept ON emp.deptid = dept.deptid
WHERE salary = (
    SELECT MAX(salary) FROM emp WHERE deptid = emp.deptid
);
```

**Output:**
```
+------+--------+----------+
| name | salary | deptname |
+------+--------+----------+
| Hank |  70000 | IT       |
+------+--------+----------+
```

---

### Q4. Show Department with the highest average salary (derived table)

**Query:**
```sql
SELECT deptname, avgSalary
FROM (
    SELECT deptname, AVG(salary) AS avgSalary
    FROM emp
    JOIN dept ON emp.deptid = dept.deptid
    GROUP BY deptname
) AS dept_avg
ORDER BY avgSalary DESC
LIMIT 1;
```

**Output:**
```
+----------+------------+
| deptname | avgSalary  |
+----------+------------+
| IT       | 53750.0000 |
+----------+------------+
```

---

### Q5. Show Departments having more than 3 employees (EXISTS)

**Query:**
```sql
SELECT deptname FROM dept
WHERE EXISTS (
    SELECT 1 FROM emp WHERE emp.deptid = dept.deptid
    GROUP BY deptid
    HAVING COUNT(*) > 3
);
```

**Output:**
```
+----------+
| deptname |
+----------+
| IT       |
+----------+
```

---

### Q6. Show Employees in the 'IT' department using a subquery

**Query:**
```sql
SELECT e.Name, e.Salary
FROM Emp e
WHERE DeptID = (SELECT DeptID FROM Dept WHERE DeptName = 'IT');
```

**Output:**
```
+------+--------+
| Name | Salary |
+------+--------+
| Bob  |  45000 |
| Eva  |  60000 |
| Hank |  70000 |
| Jack |  40000 |
+------+--------+
```

---

### Q7. Show Employees earning more than ALL HR salaries

**Query:**
```sql
SELECT e.Name, e.Salary
FROM Emp e
WHERE e.Salary > ALL (
    SELECT Salary FROM Emp WHERE DeptID = (SELECT DeptID FROM Dept WHERE DeptName = 'HR')
);
```

**Output:**
```
+-------+--------+
| Name  | Salary |
+-------+--------+
| Bob   |  45000 |
| David |  55000 |
| Eva   |  60000 |
| Frank |  50000 |
| Hank  |  70000 |
| Ivy   |  45000 |
+-------+--------+
```

---

### Q8. List departments with no employees (anti-exists)

**Query:**
```sql
SELECT d.DeptName
FROM Dept d
WHERE NOT EXISTS (
    SELECT 1 FROM Emp e WHERE e.DeptID = d.DeptID
);
```

**Output:**
```
Empty set (no departments without employees)
```

---

### Q9. Employees who share the same salary with someone else in the same department

**Query:**
```sql
SELECT e1.Name, e1.Salary, d.DeptName
FROM Emp e1
JOIN Dept d ON e1.DeptID = d.DeptID
WHERE EXISTS (
    SELECT 1
    FROM Emp e2
    WHERE e2.DeptID = e1.DeptID
      AND e2.Salary = e1.Salary
      AND e2.EmpID <> e1.EmpID
);
```

**Output:**
```
Empty set (no employees share the same salary in the same department)
```

---

### Q10. Second highest salary overall

**Query:**
```sql
SELECT MAX(Salary) AS SecondHighestSalary
FROM Emp
WHERE Salary < (SELECT MAX(Salary) FROM Emp);
```

**Output:**
```
+---------------------+
| SecondHighestSalary |
+---------------------+
|               60000 |
+---------------------+
```

---

### Q11. Top 2 earners per department (Window Function - MySQL 8+)

**Query:**
```sql
SELECT EmpID, Name, Salary, DeptName
FROM (
    SELECT e.*, d.DeptName,
           ROW_NUMBER() OVER (PARTITION BY e.DeptID ORDER BY Salary DESC) AS rn
    FROM Emp e
    JOIN Dept d ON e.DeptID = d.DeptID
) AS t
WHERE rn <= 2;
```

**Output:**
```
+-------+---------+--------+----------+
| EmpID | Name    | Salary | DeptName |
+-------+---------+--------+----------+
|   103 | Charlie |  40000 | HR       |
|   107 | Grace   |  35000 | HR       |
|   108 | Hank    |  70000 | IT       |
|   105 | Eva     |  60000 | IT       |
|   104 | David   |  55000 | Finance  |
|   106 | Frank   |  50000 | Finance  |
+-------+---------+--------+----------+
```

---

### Q12. Find employees whose salary is within the top 3 salaries overall

**Query:**
```sql
SELECT Name, Salary
FROM Emp
ORDER BY Salary DESC
LIMIT 3;
```

**Output:**
```
+-------+--------+
| Name  | Salary |
+-------+--------+
| Hank  |  70000 |
| Eva   |  60000 |
| David |  55000 |
+-------+--------+
```

---

### Q13. Derived table: join back department aggregates (avg & max)

**Query:**
```sql
SELECT e.Name, e.Salary, d.DeptName, dept_avg.AvgSalary, dept_avg.MaxSalary
FROM Emp e
JOIN Dept d ON e.DeptID = d.DeptID
JOIN (
    SELECT DeptID, AVG(Salary) AS AvgSalary, MAX(Salary) AS MaxSalary
    FROM Emp
    GROUP BY DeptID
) AS dept_avg ON e.DeptID = dept_avg.DeptID;
```

**Output:**
```
+---------+--------+----------+------------+-----------+
| Name    | Salary | DeptName | AvgSalary  | MaxSalary |
+---------+--------+----------+------------+-----------+
| Alice   |  30000 | HR       | 35000.0000 |     40000 |
| Charlie |  40000 | HR       | 35000.0000 |     40000 |
| Grace   |  35000 | HR       | 35000.0000 |     40000 |
| Bob     |  45000 | IT       | 53750.0000 |     70000 |
| Eva     |  60000 | IT       | 53750.0000 |     70000 |
| Hank    |  70000 | IT       | 53750.0000 |     70000 |
| Jack    |  40000 | IT       | 53750.0000 |     70000 |
| David   |  55000 | Finance  | 50000.0000 |     55000 |
| Frank   |  50000 | Finance  | 50000.0000 |     55000 |
| Ivy     |  45000 | Finance  | 50000.0000 |     55000 |
+---------+--------+----------+------------+-----------+
```

---

### Q14. Employees whose salary is below (dept minimum + 5000)

**Query:**
```sql
SELECT e.Name, e.Salary, d.DeptName
FROM Emp e
JOIN Dept d ON e.DeptID = d.DeptID
WHERE e.Salary < (
    SELECT MIN(Salary) + 5000
    FROM Emp
    WHERE DeptID = e.DeptID
);
```

**Output:**
```
+-------+--------+----------+
| Name  | Salary | DeptName |
+-------+--------+----------+
| Alice |  30000 | HR       |
| Jack  |  40000 | IT       |
| Ivy   |  45000 | Finance  |
+-------+--------+----------+
```

---

### Q15. Department(s) where all employees earn at least 45,000

**Query:**
```sql
SELECT d.DeptName
FROM Dept d
WHERE NOT EXISTS (
    SELECT 1
    FROM Emp e
    WHERE e.DeptID = d.DeptID
      AND e.Salary < 45000
);
```

**Output:**
```
+----------+
| DeptName |
+----------+
| Finance  |
+----------+
```

---

## Key Concepts Summary

### Aggregate Functions
- `SUM()` - Total of values
- `AVG()` - Average of values
- `COUNT()` - Number of rows
- `MAX()` - Maximum value
- `MIN()` - Minimum value

### Clauses
- `GROUP BY` - Groups rows with same values
- `HAVING` - Filters groups (after aggregation)
- `WHERE` - Filters rows (before aggregation)
- `ORDER BY` - Sorts results
- `LIMIT` - Restricts number of rows

### Subquery Types
- **Scalar**: Returns single value
- **Correlated**: References outer query
- **Derived Table**: Subquery in FROM clause
- **EXISTS**: Tests for existence

### Window Functions (MySQL 8+)
- `ROW_NUMBER()` - Assigns unique row numbers
- `PARTITION BY` - Divides result set into partitions
- `OVER()` - Defines window for function

### JOIN Types
- `INNER JOIN` / `JOIN` - Matching rows from both tables
- Used with `ON` clause to specify join condition

---

## Practice Tips

1. **Always use table aliases** for clarity in joins
2. **Test subqueries independently** before nesting
3. **Use EXPLAIN** to understand query execution
4. **Remember**: WHERE filters before grouping, HAVING filters after
5. **Correlated subqueries** execute once per outer row (slower)
6. **Window functions** are powerful for ranking and analytics