# Database Assignment No-4
## PG-DAC August 2025

---

## Question 1
**Write a query to calculate the total salary of all employees.**

### SQL Command
```sql
SELECT SUM(SALARY) AS TOTAL_SALARY FROM EMPLOYEE;
```

### Output
```
+--------------+
| TOTAL_SALARY |
+--------------+
|    469500.00 |
+--------------+
```

### Concept
- **Aggregate Function - SUM()**: Calculates the sum of all values in a column
- Used to find total/cumulative values across all rows
- Returns a single value for the entire dataset

---

## Question 2
**Write a query to find the average salary of employees in each department using GROUP BY.**

### SQL Command
```sql
SELECT DEPTID, AVG(SALARY) AS AVG_SALARY 
FROM EMPLOYEE 
GROUP BY DEPTID;
```

### Output
```
+--------+--------------+
| DEPTID | AVG_SALARY   |
+--------+--------------+
|      1 | 67375.000000 |
|      2 | 60000.000000 |
|      3 | 40000.000000 |
+--------+--------------+
```

### Concept
- **GROUP BY Clause**: Groups rows with same values in specified column(s)
- **AVG() Function**: Calculates average of numeric values
- Combines grouping with aggregation to get department-wise statistics

---

## Question 3
**Write a query to count the total number of employees in each department.**

### SQL Command
```sql
SELECT DEPTID, COUNT(*) AS TOTAL_EMPLOYEES 
FROM EMPLOYEE 
GROUP BY DEPTID;
```

### Output
```
+--------+-----------------+
| DEPTID | TOTAL_EMPLOYEES |
+--------+-----------------+
|      1 |               4 |
|      2 |               2 |
|      3 |               2 |
+--------+-----------------+
```

### Concept
- **COUNT() Function**: Counts number of rows
- **COUNT(*)**: Counts all rows including NULL values
- Used with GROUP BY for category-wise counting

---

## Question 4
**Write a query to display departments having more than 5 employees using HAVING clause.**

### SQL Command
```sql
SELECT DEPTID, COUNT(*) AS TOTAL_EMPLOYEES
FROM EMPLOYEE 
GROUP BY DEPTID
HAVING COUNT(*) > 5;
```

### Output
```
Empty set (0.01 sec)
```

**Modified Query** (to show departments with more than 3 employees):
```sql
SELECT DEPTID, COUNT(*) AS TOTAL_EMPLOYEES 
FROM EMPLOYEE
GROUP BY DEPTID
HAVING COUNT(*) > 3;
```

### Output
```
+--------+-----------------+
| DEPTID | TOTAL_EMPLOYEES |
+--------+-----------------+
|      1 |               4 |
+--------+-----------------+
```

### Concept
- **HAVING Clause**: Filters groups after GROUP BY is applied
- Different from WHERE (which filters rows before grouping)
- Used with aggregate functions to filter grouped results

---

## Question 5
**Write a query to list distinct department locations from the Dept table.**

### SQL Command
```sql
-- First, add LOCATION column to DEPARTMENT table
ALTER TABLE DEPARTMENT ADD LOCATION VARCHAR(50);

-- Insert sample data
INSERT INTO DEPARTMENT(DEPTID, DEPT_NAME, LOCATION) 
VALUES(5, 'GET', 'PUNE'), (6, 'IT', 'PUNE'), (7, 'FINANCE', 'MUMABI');

-- Query distinct locations
SELECT DISTINCT LOCATION FROM DEPARTMENT;
```

### Output
```
+----------+
| LOCATION |
+----------+
| NULL     |
| PUNE     |
| MUMABI   |
+----------+
```

### Concept
- **DISTINCT Keyword**: Removes duplicate values from result set
- **ALTER TABLE**: Modifies existing table structure (DDL command)
- Returns only unique values from specified column

---

## Question 6
**Write a query to display the highest salary among all employees. Write a query to display total number of employees in table.**

### SQL Commands
```sql
-- Maximum Salary
SELECT MAX(SALARY) AS MAXIMUM_SALARY FROM EMPLOYEE;

-- Total Employee Count
SELECT COUNT(*) AS TOTAL_EMP FROM EMPLOYEE;
```

### Output
**Maximum Salary:**
```
+----------------+
| MAXIMUM_SALARY |
+----------------+
|       88000.00 |
+----------------+
```

**Total Employees:**
```
+-----------+
| TOTAL_EMP |
+-----------+
|         8 |
+-----------+
```

### Concept
- **MAX() Function**: Returns the highest value in a column
- **COUNT(*)**: Returns total number of rows in table
- Aggregate functions work on entire dataset without GROUP BY

---

## Question 7
**Write a query to find employees whose name starts with 'A' using LIKE operator.**

### SQL Command
```sql
SELECT * FROM EMPLOYEE WHERE EMPNAME LIKE 'A%';
```

### Output
```
+-------+--------------+----------+--------+-------------+-------+
| EMPID | EMPNAME      | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+--------------+----------+--------+-------------+-------+
|   102 | ANUJA GHOLAP | 70000.00 |      2 | 2024-08-09  | NULL  |
+-------+--------------+----------+--------+-------------+-------+
```

### Concept
- **LIKE Operator**: Used for pattern matching in strings
- **'A%'**: Matches any string starting with 'A' (% is wildcard for any characters)
- Case sensitivity depends on database collation

---

## Question 8
**Write a query to find employees whose name ends with 'n' using LIKE operator.**

### SQL Command
```sql
SELECT * FROM EMPLOYEE WHERE EMPNAME LIKE '%n';
```

### Output
```
Empty set (0.00 sec)
```

**Modified Query** (ending with 'L'):
```sql
SELECT * FROM EMPLOYEE WHERE EMPNAME LIKE '%L';
```

### Output
```
+-------+---------------+----------+--------+-------------+-------+
| EMPID | EMPNAME       | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+---------------+----------+--------+-------------+-------+
|   109 | VRUNDA BATWAL | 40000.00 |      3 | 2025-01-02  | NULL  |
+-------+---------------+----------+--------+-------------+-------+
```

### Concept
- **'%n'**: Matches any string ending with 'n'
- **%**: Represents zero or more characters
- Useful for suffix-based searches

---

## Question 9
**Write a query to find employees whose name contains 'ra' using LIKE operator.**

### SQL Command
```sql
SELECT * FROM EMPLOYEE WHERE EMPNAME LIKE '%ra%';
```

### Output
```
Empty set (0.00 sec)
```

**Modified Query** (containing 'R' and 'P'):
```sql
SELECT * FROM EMPLOYEE WHERE EMPNAME LIKE '%R%P';
```

### Output
```
+-------+---------------+----------+--------+-------------+-------+
| EMPID | EMPNAME       | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+---------------+----------+--------+-------------+-------+
|   101 | RUTUJA GHOLAP | 88000.00 |      1 | 2025-02-01  | NULL  |
+-------+---------------+----------+--------+-------------+-------+
```

### Concept
- **'%ra%'**: Matches any string containing 'ra' anywhere
- Pattern matching is case-sensitive in some databases
- Multiple % wildcards can be used for complex patterns

---

## Question 10
**Write a query to display all employees sorted by their Salary in descending order.**

### SQL Command
```sql
SELECT * FROM EMPLOYEE ORDER BY SALARY DESC;
```

### Output
```
+-------+---------------------+----------+--------+-------------+-------+
| EMPID | EMPNAME             | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+---------------------+----------+--------+-------------+-------+
|   101 | RUTUJA GHOLAP       | 88000.00 |      1 | 2025-02-01  | NULL  |
|   107 | DNYANESHWARI        | 88000.00 |      1 | 2024-08-08  | NULL  |
|   102 | ANUJA GHOLAP        | 70000.00 |      2 | 2024-08-09  | NULL  |
|   103 | PRERNA DIWATE       | 55000.00 |      1 | 2025-09-06  | NULL  |
|   108 | KANCHAN SURYAVANSHI | 50000.00 |      2 | 2025-09-04  | NULL  |
|   104 | SANIKA SHINDE       | 40000.00 |      3 | 2025-01-01  | NULL  |
|   109 | VRUNDA BATWAL       | 40000.00 |      3 | 2025-01-02  | NULL  |
|   106 | SUMIT DHIVAR        | 38500.00 |      1 | 2025-09-01  | NULL  |
+-------+---------------------+----------+--------+-------------+-------+
```

### Concept
- **ORDER BY Clause**: Sorts result set by specified column(s)
- **DESC**: Descending order (highest to lowest)
- **ASC**: Ascending order (default, lowest to highest)

---

## Question 11
**Write a query to display all employees sorted by DeptID ascending and then Salary descending.**

### SQL Command
```sql
SELECT * FROM EMPLOYEE ORDER BY DEPTID ASC, SALARY DESC;
```

### Output
```
+-------+---------------------+----------+--------+-------------+-------+
| EMPID | EMPNAME             | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+---------------------+----------+--------+-------------+-------+
|   101 | RUTUJA GHOLAP       | 88000.00 |      1 | 2025-02-01  | NULL  |
|   107 | DNYANESHWARI        | 88000.00 |      1 | 2024-08-08  | NULL  |
|   103 | PRERNA DIWATE       | 55000.00 |      1 | 2025-09-06  | NULL  |
|   106 | SUMIT DHIVAR        | 38500.00 |      1 | 2025-09-01  | NULL  |
|   102 | ANUJA GHOLAP        | 70000.00 |      2 | 2024-08-09  | NULL  |
|   108 | KANCHAN SURYAVANSHI | 50000.00 |      2 | 2025-09-04  | NULL  |
|   104 | SANIKA SHINDE       | 40000.00 |      3 | 2025-01-01  | NULL  |
|   109 | VRUNDA BATWAL       | 40000.00 |      3 | 2025-01-02  | NULL  |
+-------+---------------------+----------+--------+-------------+-------+
```

### Concept
- **Multiple Column Sorting**: First sorts by DEPTID, then by SALARY within each department
- Primary sort: DEPTID ascending
- Secondary sort: SALARY descending
- Useful for hierarchical data organization

---

## Question 12
**Write a query to find employees whose salary is between 30,000 and 60,000.**

### SQL Command
```sql
SELECT * FROM EMPLOYEE WHERE SALARY BETWEEN 30000 AND 60000;
```

### Output
```
+-------+---------------------+----------+--------+-------------+-------+
| EMPID | EMPNAME             | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+---------------------+----------+--------+-------------+-------+
|   103 | PRERNA DIWATE       | 55000.00 |      1 | 2025-09-06  | NULL  |
|   104 | SANIKA SHINDE       | 40000.00 |      3 | 2025-01-01  | NULL  |
|   106 | SUMIT DHIVAR        | 38500.00 |      1 | 2025-09-01  | NULL  |
|   108 | KANCHAN SURYAVANSHI | 50000.00 |      2 | 2025-09-04  | NULL  |
|   109 | VRUNDA BATWAL       | 40000.00 |      3 | 2025-01-02  | NULL  |
+-------+---------------------+----------+--------+-------------+-------+
```

### Concept
- **BETWEEN Operator**: Selects values within a given range (inclusive)
- Equivalent to: `SALARY >= 30000 AND SALARY <= 60000`
- Works with numbers, dates, and text

---

## Question 13
**Write a query to display all employees whose DeptID is in (10, 20, 30).**

### SQL Command
```sql
SELECT * FROM EMPLOYEE WHERE DEPTID IN(1, 2, 5);
```

### Output
```
+-------+---------------------+----------+--------+-------------+-------+
| EMPID | EMPNAME             | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+---------------------+----------+--------+-------------+-------+
|   101 | RUTUJA GHOLAP       | 88000.00 |      1 | 2025-02-01  | NULL  |
|   102 | ANUJA GHOLAP        | 70000.00 |      2 | 2024-08-09  | NULL  |
|   103 | PRERNA DIWATE       | 55000.00 |      1 | 2025-09-06  | NULL  |
|   106 | SUMIT DHIVAR        | 38500.00 |      1 | 2025-09-01  | NULL  |
|   107 | DNYANESHWARI        | 88000.00 |      1 | 2024-08-08  | NULL  |
|   108 | KANCHAN SURYAVANSHI | 50000.00 |      2 | 2025-09-04  | NULL  |
+-------+---------------------+----------+--------+-------------+-------+
```

### Concept
- **IN Operator**: Matches any value in a list
- Shorthand for multiple OR conditions
- More readable than: `DEPTID = 1 OR DEPTID = 2 OR DEPTID = 5`

---

## Question 14
**Write a query to display Min salary of employee.**

### SQL Command
```sql
SELECT MIN(SALARY) AS MINIMUM_SALARY FROM EMPLOYEE;
```

### Output
```
+----------------+
| MINIMUM_SALARY |
+----------------+
|       38500.00 |
+----------------+
```

### Concept
- **MIN() Function**: Returns the smallest value in a column
- Aggregate function that works on numeric, date, or text columns
- Useful for finding lowest values in datasets

---

## Question 15
**Write a query to display employees whose JoiningDate is between '2020-01-01' and '2021-12-31'.**

### SQL Command
```sql
SELECT * FROM EMPLOYEE 
WHERE JOININGDATE BETWEEN '2020-01-01' AND '2021-12-31';
```

### Output
```
Empty set (0.00 sec)
```

**Modified Query** (for available date range):
```sql
SELECT * FROM EMPLOYEE 
WHERE JOININGDATE BETWEEN '2020-01-01' AND '2024-09-09';
```

### Output
```
+-------+--------------+----------+--------+-------------+-------+
| EMPID | EMPNAME      | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+--------------+----------+--------+-------------+-------+
|   102 | ANUJA GHOLAP | 70000.00 |      2 | 2024-08-09  | NULL  |
|   107 | DNYANESHWARI | 88000.00 |      1 | 2024-08-08  | NULL  |
+-------+--------------+----------+--------+-------------+-------+
```

### Concept
- **BETWEEN with Dates**: Works with date ranges
- Date format: 'YYYY-MM-DD' (ISO standard)
- Inclusive of both start and end dates

---

## Question 16
**Write a query to display employees whose Salary is NULL.**

### SQL Command
```sql
SELECT * FROM EMPLOYEE WHERE SALARY IS NULL;
```

### Output
```
Empty set (0.00 sec)
```

### Concept
- **IS NULL**: Checks for NULL values
- NULL represents missing or unknown data
- Cannot use `= NULL` (must use `IS NULL`)

---

## Question 17
**Write a query to display employees whose Salary is NOT NULL.**

### SQL Command
```sql
SELECT * FROM EMPLOYEE WHERE SALARY IS NOT NULL;
```

### Output
```
+-------+---------------------+----------+--------+-------------+-------+
| EMPID | EMPNAME             | SALARY   | DEPTID | JOININGDATE | EMAIL |
+-------+---------------------+----------+--------+-------------+-------+
|   101 | RUTUJA GHOLAP       | 88000.00 |      1 | 2025-02-01  | NULL  |
|   102 | ANUJA GHOLAP        | 70000.00 |      2 | 2024-08-09  | NULL  |
|   103 | PRERNA DIWATE       | 55000.00 |      1 | 2025-09-06  | NULL  |
|   104 | SANIKA SHINDE       | 40000.00 |      3 | 2025-01-01  | NULL  |
|   106 | SUMIT DHIVAR        | 38500.00 |      1 | 2025-09-01  | NULL  |
|   107 | DNYANESHWARI        | 88000.00 |      1 | 2024-08-08  | NULL  |
|   108 | KANCHAN SURYAVANSHI | 50000.00 |      2 | 2025-09-04  | NULL  |
|   109 | VRUNDA BATWAL       | 40000.00 |      3 | 2025-01-02  | NULL  |
+-------+---------------------+----------+--------+-------------+-------+
```

### Concept
- **IS NOT NULL**: Filters out NULL values
- Returns only rows with actual data in specified column
- Essential for data quality checks

---

## Question 18
**Write a query to calculate the total salary per department, but only for departments where total salary is greater than 1,00,000 (use HAVING).**

### SQL Command
```sql
SELECT DEPTID, SUM(SALARY) AS TOTAL_SALARY
FROM EMPLOYEE
GROUP BY DEPTID
HAVING SUM(SALARY) > 100000;
```

### Output
```
+--------+--------------+
| DEPTID | TOTAL_SALARY |
+--------+--------------+
|      1 |    269500.00 |
|      2 |    120000.00 |
+--------+--------------+
```

### Concept
- **HAVING with Aggregate Functions**: Filters grouped results
- Applied after GROUP BY (unlike WHERE which is before)
- Used specifically for filtering aggregate function results

---

## Question 19
**Write a query to display all distinct employee names.**

### SQL Command
```sql
SELECT DISTINCT EMPNAME FROM EMPLOYEE;
```

### Output
```
+---------------------+
| EMPNAME             |
+---------------------+
| RUTUJA GHOLAP       |
| ANUJA GHOLAP        |
| PRERNA DIWATE       |
| SANIKA SHINDE       |
| SUMIT DHIVAR        |
| DNYANESHWARI        |
| KANCHAN SURYAVANSHI |
| VRUNDA BATWAL       |
+---------------------+
```

### Concept
- **DISTINCT on Text Columns**: Removes duplicate names
- Useful for getting unique values from any column
- Can be applied to multiple columns simultaneously

---

## Question 20
**Write a query to count the number of departments having the same location.**

### SQL Command
```sql
SELECT LOCATION, COUNT(*) AS TOTAL_DEPT
FROM DEPARTMENT
GROUP BY LOCATION;
```

### Output
```
+----------+------------+
| LOCATION | TOTAL_DEPT |
+----------+------------+
| NULL     |          3 |
| PUNE     |          1 |
| MUMABI   |          1 |
+----------+------------+
```

### Concept
- **GROUP BY with COUNT**: Counts occurrences of each unique value
- Groups records by location and counts departments in each
- NULL values are treated as a separate group

---

## Summary of Key SQL Concepts

### Aggregate Functions
- **SUM()**: Total of all values
- **AVG()**: Average of values
- **COUNT()**: Number of rows
- **MAX()**: Highest value
- **MIN()**: Lowest value

### Clauses
- **WHERE**: Filters rows before grouping
- **GROUP BY**: Groups rows with same values
- **HAVING**: Filters groups after aggregation
- **ORDER BY**: Sorts result set

### Operators
- **LIKE**: Pattern matching (%, _)
- **BETWEEN**: Range selection (inclusive)
- **IN**: Match any value in list
- **IS NULL / IS NOT NULL**: Check for NULL values

### Keywords
- **DISTINCT**: Remove duplicates
- **ASC/DESC**: Sort order

---
