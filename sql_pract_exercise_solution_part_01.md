# Employee Database SQL Exercises

Database: `employee`
Table: `Employee`

```sql
CREATE TABLE Employee (
    EmpID INT PRIMARY KEY,
    Name VARCHAR(50),
    Dept VARCHAR(50),
    Salary INT
);

INSERT INTO Employee VALUES
(1, 'Alice',   'HR',      30000),
(2, 'Bob',     'IT',      45000),
(3, 'Charlie', 'HR',      40000),
(4, 'David',   'Finance', 55000),
(5, 'Eva',     'IT',      60000),
(6, 'Frank',   'Finance', 50000),
(7, 'Grace',   'HR',      35000),
(8, 'Hank',    'IT',      70000),
(9, 'Ivy',     'Finance', 45000),
(10,'Jack',    'IT',      40000);
```

---

## 1. Group By & Having Clause

### Q1. Find the total salary paid in each department

```sql
SELECT Dept, SUM(Salary) AS total_salary
FROM Employee
GROUP BY Dept;
```

Output:
| Dept    | total_salary |
|---------|--------------|
| HR      | 105000       |
| IT      | 215000       |
| Finance | 150000       |

### Q2. Find the average salary of each department

```sql
SELECT Dept, AVG(Salary) AS average_salary
FROM Employee
GROUP BY Dept;
```

Output:
| Dept    | average_salary |
|---------|----------------|
| HR      | 35000.0000     |
| IT      | 53750.0000     |
| Finance | 50000.0000     |

### Q3. Find departments where total salary is more than 150000

```sql
SELECT Dept
FROM Employee
GROUP BY Dept
HAVING SUM(Salary) > 150000;
```

Output:
| Dept |
|------|
| IT   |

### Q4. Find departments having more than 2 employees

```sql
SELECT Dept
FROM Employee
GROUP BY Dept
HAVING COUNT(EmpID) > 2;
```

Output:
| Dept    |
|---------|
| HR      |
| IT      |
| Finance |

### Q5. Find the highest salary in each department

```sql
SELECT Dept, MAX(Salary) AS max_salary
FROM Employee
GROUP BY Dept;
```

Output:
| Dept    | max_salary |
|---------|------------|
| HR      | 45000      |
| IT      | 70000      |
| Finance | 55000      |

### Q6. Find departments where the average salary is greater than 40000

```sql
SELECT Dept
FROM Employee
GROUP BY Dept
HAVING AVG(Salary) > 40000;
```

Output:
| Dept    |
|---------|
| IT      |
| Finance |

### Q7. Find the number of employees in each department, but only show departments with total salary > 100000

```sql
SELECT Dept, COUNT(EmpID) AS num_emp, SUM(Salary) AS total_salary
FROM Employee
GROUP BY Dept
HAVING SUM(Salary) > 100000;
```

Output:
| Dept    | num_emp | total_salary |
|---------|---------|--------------|
| HR      | 3       | 105000       |
| IT      | 4       | 215000       |
| Finance | 3       | 150000       |

---

## 2. Views

### Q1. Create a simple view of all IT employees & Query it

```sql
CREATE VIEW it_employees AS
SELECT EmpID, Name, Dept, Salary
FROM Employee
WHERE Dept = 'IT';

SELECT * FROM it_employees;
```

Output:
| EmpID | Name  | Dept | Salary |
|-------|-------|------|--------|
| 2     | Bob   | IT   | 45000  |
| 5     | Eva   | IT   | 60000  |
| 8     | Hank  | IT   | 70000  |
| 10    | Jack  | IT   | 40000  |

### Q2. Create an aggregate view with department stats

```sql
CREATE OR REPLACE VIEW dept_status AS
SELECT
    Dept,
    COUNT(EmpID) AS num_employees,
    SUM(Salary) AS total_salary,
    AVG(Salary) AS avg_salary,
    MAX(Salary) AS max_salary,
    MIN(Salary) AS min_salary
FROM Employee
GROUP BY Dept;

SELECT * FROM dept_status;
```

Output:
| Dept    | num_employees | total_salary | avg_salary | max_salary | min_salary |
|---------|---------------|--------------|------------|------------|------------|
| HR      | 3             | 105000       | 35000.0000 | 45000      | 30000      |
| IT      | 4             | 215000       | 53750.0000 | 70000      | 40000      |
| Finance | 3             | 150000       | 50000.0000 | 55000      | 45000      |

### Q3. Updatable view for low-paid HR employees & Update salary by 10000

```sql
CREATE OR REPLACE VIEW lowPaid_HR AS
SELECT EmpID, Name, Dept, Salary
FROM Employee
WHERE Dept = 'HR';

UPDATE lowPaid_HR
SET Salary = Salary + 10000;

SELECT * FROM Employee WHERE Dept = 'HR';
```

Output:
| EmpID | Name    | Dept | Salary |
|-------|---------|------|--------|
| 1     | Alice   | HR   | 40000  |
| 3     | Charlie | HR   | 40000  |
| 7     | Grace   | HR   | 45000  |

### Q4. Create a view with computed column Salary Band

```sql
CREATE VIEW Salary_Band_View AS
SELECT EmpID, Name, Dept, Salary,
       CASE
           WHEN Salary > 60000 THEN 'High Salary'
           WHEN Salary > 40000 THEN 'Mid Salary'
           ELSE 'Low Salary'
       END AS salary_band
FROM Employee;

SELECT * FROM Salary_Band_View;
```

Output:
| EmpID | Name    | Dept    | Salary | salary_band |
|-------|---------|---------|--------|-------------|
| 1     | Alice   | HR      | 40000  | Low Salary  |
| 2     | Bob     | IT      | 45000  | Mid Salary  |
| 3     | Charlie | HR      | 40000  | Low Salary  |
| 4     | David   | Finance | 55000  | Mid Salary  |
| 5     | Eva     | IT      | 60000  | Mid Salary  |
| 6     | Frank   | Finance | 50000  | Mid Salary  |
| 7     | Grace   | HR      | 45000  | Mid Salary  |
| 8     | Hank    | IT      | 70000  | High Salary |
| 9     | Ivy     | Finance | 45000  | Mid Salary  |
| 10    | Jack    | IT      | 40000  | Low Salary  |

### Q5. Replace view

Example done using `CREATE OR REPLACE VIEW` above.

### Q6. Drop view

```sql
DROP VIEW IF EXISTS it_employees;
```

---

## 3. Subqueries

### Q01. Single-row subquery: Find the second highest salary overall

```sql
SELECT MAX(Salary) AS second_highest_salary
FROM Employee
WHERE Salary < (SELECT MAX(Salary) FROM Employee);
```

Output:
| second_highest_salary |
|----------------------|
| 60000                |

### Q02. Single-row subquery per dept: Find Highest salary in HR

```sql
SELECT MAX(Salary) AS highest_hr_salary
FROM Employee
WHERE Dept = 'HR';
```

Output:
| highest_hr_salary |
|------------------|
| 45000            |

