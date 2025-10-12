# MySQL Assignment 6 - Stored Procedures, Functions, Triggers & Cursors

---

## 1. Stored Procedure Without Parameters

### Question
Write a SQL query to create a stored procedure without any parameters that displays all employees from the Emp table.

### Concept
A **stored procedure** is a precompiled set of SQL statements stored in the database. Procedures without parameters are the simplest form and can be called directly without passing any values.

**Syntax:**
```sql
DELIMITER $$
CREATE PROCEDURE procedure_name()
BEGIN
    -- SQL statements
END $$
DELIMITER ;
```

### Solution
```sql
DELIMITER $$
CREATE PROCEDURE DisplayAllEmployees()
BEGIN
    SELECT * FROM Emp;
END $$
DELIMITER ;
```

### Execution
```sql
CALL DisplayAllEmployees();
```

### Output
```
+-------+---------------+--------------+----------+------------+
| EmpID | EmpName       | DepartmentID | Salary   | HireDate   |
+-------+---------------+--------------+----------+------------+
|     1 | John Doe      |           10 | 90000.00 | 2020-01-10 |
|     2 | Alice Smith   |           20 | 75000.00 | 2019-03-15 |
|     3 | Bob Johnson   |           10 | 48000.00 | 2021-06-21 |
|     4 | Charlie Brown |           30 | 85000.00 | 2022-04-11 |
|     5 | David Lee     |           20 | 25000.00 | 2018-08-30 |
|     6 | Emily Davis   |           30 | 55000.00 | 2023-02-17 |
|     7 | Frank Wilson  |           10 | 62000.00 | 2020-09-09 |
|     8 | Grace Taylor  |           40 | 95000.00 | 2021-11-22 |
+-------+---------------+--------------+----------+------------+
```

---

## 2. Stored Procedure with IN Parameter

### Question
Write a SQL query to create a stored procedure with an IN parameter that accepts a department ID and displays all employees belonging to that department.

### Concept
**IN parameters** allow you to pass values into a stored procedure. These parameters act as input variables that can be used within the procedure logic.

**Syntax:**
```sql
CREATE PROCEDURE procedure_name(IN parameter_name datatype)
BEGIN
    -- Use parameter_name in SQL statements
END;
```

### Solution
```sql
DELIMITER $$
CREATE PROCEDURE GetEmployeesByDept(IN dept_id INT)
BEGIN
    SELECT * FROM Emp
    WHERE DepartmentID = dept_id;
END $$
DELIMITER ;
```

### Execution
```sql
CALL GetEmployeesByDept(10);
```

### Output
```
+-------+--------------+--------------+----------+------------+
| EmpID | EmpName      | DepartmentID | Salary   | HireDate   |
+-------+--------------+--------------+----------+------------+
|     1 | John Doe     |           10 | 90000.00 | 2020-01-10 |
|     3 | Bob Johnson  |           10 | 48000.00 | 2021-06-21 |
|     7 | Frank Wilson |           10 | 62000.00 | 2020-09-09 |
+-------+--------------+--------------+----------+------------+
```

---

## 3. Stored Procedure with OUT Parameter

### Question
Write a SQL query to create a stored procedure with an OUT parameter that returns the total number of employees in the Emp table.

### Concept
**OUT parameters** are used to return values from a stored procedure. The procedure sets the value of the OUT parameter, which can then be retrieved by the calling program.

**Syntax:**
```sql
CREATE PROCEDURE procedure_name(OUT parameter_name datatype)
BEGIN
    SELECT column INTO parameter_name FROM table;
END;
```

### Solution
```sql
DELIMITER $$
CREATE PROCEDURE GetTotalEmployees(OUT total INT)
BEGIN
    SELECT COUNT(*) INTO total FROM Emp;
END $$
DELIMITER ;
```

### Execution
```sql
CALL GetTotalEmployees(@count);
SELECT @count AS TotalEmployees;
```

### Output
```
+----------------+
| TotalEmployees |
+----------------+
|              8 |
+----------------+
```

---

## 4. Function with IF-ELSE for Salary Grading

### Question
Write a SQL function that accepts an employee's salary as input and returns a grade based on the following conditions:
- If salary ≥ 80,000 → Grade = 'A'
- If salary ≥ 50,000 and < 80,000 → Grade = 'B'
- If salary ≥ 30,000 and < 50,000 → Grade = 'C'
- Otherwise → Grade = 'D'

### Concept
**User-Defined Functions (UDF)** return a single value and can be used in SQL expressions. The `DETERMINISTIC` keyword indicates that the function always produces the same result for the same input parameters.

**Key Points:**
- Functions must have a `RETURNS` clause
- Must include a `RETURN` statement
- Can use IF-ELSE or CASE statements for conditional logic

**Syntax:**
```sql
CREATE FUNCTION function_name(parameter datatype)
RETURNS return_datatype
DETERMINISTIC
BEGIN
    DECLARE variable datatype;
    -- Logic
    RETURN variable;
END;
```

### Solution
```sql
DELIMITER $$
CREATE FUNCTION GetGrade(salary DECIMAL(10,2))
RETURNS CHAR(1)
DETERMINISTIC
BEGIN
    DECLARE grade CHAR(1);
    IF salary >= 80000 THEN
        SET grade = 'A';
    ELSEIF salary >= 50000 THEN
        SET grade = 'B';
    ELSEIF salary >= 30000 THEN
        SET grade = 'C';
    ELSE
        SET grade = 'D';
    END IF;
    RETURN grade;
END $$
DELIMITER ;
```

### Execution
```sql
SELECT EmpName, Salary, GetGrade(Salary) AS Grade FROM Emp;
```

### Output
```
+---------------+----------+-------+
| EmpName       | Salary   | Grade |
+---------------+----------+-------+
| John Doe      | 90000.00 | A     |
| Alice Smith   | 75000.00 | B     |
| Bob Johnson   | 48000.00 | C     |
| Charlie Brown | 85000.00 | A     |
| David Lee     | 25000.00 | D     |
| Emily Davis   | 55000.00 | B     |
| Frank Wilson  | 62000.00 | B     |
| Grace Taylor  | 95000.00 | A     |
+---------------+----------+-------+
```

---

## 5. Stored Procedure with Explicit Cursor

### Question
Write a stored procedure that uses an explicit cursor to fetch and display the details of all employees whose salary is greater than 60,000 from the Emp table.

### Concept
A **cursor** is a database object used to retrieve and manipulate data row by row. An explicit cursor gives you complete control over the cursor operations.

**Cursor Operations:**
1. **DECLARE** - Define the cursor with a SELECT statement
2. **OPEN** - Execute the query and create the result set
3. **FETCH** - Retrieve one row at a time into variables
4. **CLOSE** - Release the cursor resources

**Handler:** The `CONTINUE HANDLER FOR NOT FOUND` sets a flag when no more rows are available.

### Solution
```sql
DELIMITER $$
CREATE PROCEDURE ShowHighSalaryEmployees()
BEGIN
    DECLARE done INT DEFAULT 0;
    DECLARE e_id INT;
    DECLARE e_name VARCHAR(50);
    DECLARE e_salary DECIMAL(10,2);
    
    -- Declare cursor
    DECLARE emp_cursor CURSOR FOR
        SELECT EmpID, EmpName, Salary FROM Emp WHERE Salary > 60000;
    
    -- Declare handler for end of cursor
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;
    
    -- Open cursor
    OPEN emp_cursor;
    
    -- Loop through cursor
    read_loop: LOOP
        FETCH emp_cursor INTO e_id, e_name, e_salary;
        IF done THEN
            LEAVE read_loop;
        END IF;
        SELECT e_id AS ID, e_name AS Name, e_salary AS Salary;
    END LOOP;
    
    -- Close cursor
    CLOSE emp_cursor;
END $$
DELIMITER ;
```

### Execution
```sql
CALL ShowHighSalaryEmployees();
```

### Output
```
+------+----------+----------+
| ID   | Name     | Salary   |
+------+----------+----------+
|    1 | John Doe | 90000.00 |
+------+----------+----------+

+------+-------------+----------+
| ID   | Name        | Salary   |
+------+-------------+----------+
|    2 | Alice Smith | 75000.00 |
+------+-------------+----------+

+------+---------------+----------+
| ID   | Name          | Salary   |
+------+---------------+----------+
|    4 | Charlie Brown | 85000.00 |
+------+---------------+----------+

+------+--------------+----------+
| ID   | Name         | Salary   |
+------+--------------+----------+
|    7 | Frank Wilson | 62000.00 |
+------+--------------+----------+

+------+--------------+----------+
| ID   | Name         | Salary   |
+------+--------------+----------+
|    8 | Grace Taylor | 95000.00 |
+------+--------------+----------+
```

---

## 6. BEFORE INSERT Trigger with Validation

### Question
Write a trigger on the Emp table that checks before inserting a new employee record. If the Salary is less than 10,000, prevent the insertion and raise an error message "Salary too low".

### Concept
A **trigger** is a stored procedure that automatically executes when a specific event occurs on a table (INSERT, UPDATE, or DELETE).

**Trigger Types:**
- **BEFORE trigger** - Executes before the DML operation
- **AFTER trigger** - Executes after the DML operation

**NEW and OLD keywords:**
- `NEW` - Refers to the new row being inserted/updated
- `OLD` - Refers to the existing row before update/delete

**SIGNAL statement:** Used to raise custom errors in MySQL.

### Solution
```sql
DELIMITER $$
CREATE TRIGGER checkSalaryBeforeInsert
BEFORE INSERT ON Emp
FOR EACH ROW
BEGIN
    IF NEW.Salary < 10000 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Salary too low';
    END IF;
END $$
DELIMITER ;
```

### Testing the Trigger
```sql
INSERT INTO Emp(EmpID, EmpName, Salary) VALUES(101, 'John', 8000);
```

### Output
```
ERROR 1644 (45000): Salary too low
```

**Explanation:** The trigger prevents the insertion because the salary (8000) is less than 10,000.

---

## 7. WHILE Loop - Print Numbers 1 to 10

### Question
Write a stored procedure in SQL to print numbers from 1 to 10 using a WHILE loop.

### Concept
The **WHILE loop** repeatedly executes a block of statements as long as the specified condition is true.

**Syntax:**
```sql
WHILE condition DO
    -- statements
END WHILE;
```

**Loop Control:**
- Initialize counter variable before loop
- Update counter inside loop
- Condition checked before each iteration

### Solution
```sql
DELIMITER $$
CREATE PROCEDURE printNumbers()
BEGIN
    DECLARE i INT DEFAULT 1;
    WHILE i <= 10 DO
        SELECT i;
        SET i = i + 1;
    END WHILE;
END $$
DELIMITER ;
```

### Execution
```sql
CALL printNumbers();
```

### Output
```
+------+
| i    |
+------+
|    1 |
+------+

+------+
| i    |
+------+
|    2 |
+------+

... (continues for 3 through 10)

+------+
| i    |
+------+
|   10 |
+------+
```

---

## 8. Multiplication Table Using Loop

### Question
Write a stored procedure to print the multiplication table of 2 using a loop.

### Concept
This demonstrates using loops with string concatenation to create formatted output. The `CONCAT()` function combines multiple values into a single string.

### Solution
```sql
DELIMITER $$
CREATE PROCEDURE TableOfTwo()
BEGIN
    DECLARE i INT DEFAULT 1;
    WHILE i <= 10 DO
        SELECT CONCAT('2 X ', i, ' = ', 2 * i) AS Result;
        SET i = i + 1;
    END WHILE;
END $$
DELIMITER ;
```

### Execution
```sql
CALL TableOfTwo();
```

### Output
```
+-----------+
| Result    |
+-----------+
| 2 X 1 = 2 |
+-----------+

+-----------+
| Result    |
+-----------+
| 2 X 2 = 4 |
+-----------+

... (continues through 2 X 10 = 20)

+-------------+
| Result      |
+-------------+
| 2 X 10 = 20 |
+-------------+
```

---

## 9. Function to Check Even or Odd

### Question
Write a function to check whether a number is even or odd.

### Concept
The **MOD() function** returns the remainder of division. A number is even if `MOD(num, 2) = 0`, otherwise it's odd.

**Formula:**
- Even: num % 2 = 0
- Odd: num % 2 ≠ 0

### Solution
```sql
DELIMITER $$
CREATE FUNCTION CheckEvenOdd(num INT)
RETURNS VARCHAR(10)
DETERMINISTIC
BEGIN
    IF MOD(num, 2) = 0 THEN
        RETURN 'Even';
    ELSE
        RETURN 'Odd';
    END IF;
END $$
DELIMITER ;
```

### Execution
```sql
SELECT CheckEvenOdd(5);
SELECT CheckEvenOdd(10);
```

### Output
```
+-----------------+
| CheckEvenOdd(5) |
+-----------------+
| Odd             |
+-----------------+

+------------------+
| CheckEvenOdd(10) |
+------------------+
| Even             |
+------------------+
```

---

## 10. Function to Calculate Factorial

### Question
Write a user-defined function to calculate the factorial of a given number.

### Concept
**Factorial** is the product of all positive integers less than or equal to n.
- Formula: n! = n × (n-1) × (n-2) × ... × 2 × 1
- Example: 5! = 5 × 4 × 3 × 2 × 1 = 120

This function uses an iterative approach with a WHILE loop to calculate the factorial.

### Solution
```sql
DELIMITER $$
CREATE FUNCTION GetFactorial(n INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE result INT DEFAULT 1;
    DECLARE i INT DEFAULT 1;
    
    WHILE i <= n DO
        SET result = result * i;
        SET i = i + 1;
    END WHILE;
    
    RETURN result;
END $$
DELIMITER ;
```

### Execution
```sql
SELECT GetFactorial(5);
```

### Output
```
+-----------------+
| GetFactorial(5) |
+-----------------+
|             120 |
+-----------------+
```

**Calculation:** 5! = 1 × 2 × 3 × 4 × 5 = 120

---

## Summary

### Stored Procedures vs Functions

| Feature | Stored Procedure | Function |
|---------|-----------------|----------|
| Return Value | Can return multiple values via OUT parameters | Returns single value |
| Usage | Called using CALL statement | Used in SQL expressions |
| DML Operations | Can perform INSERT, UPDATE, DELETE | Generally read-only operations |
| Transaction Control | Can use COMMIT, ROLLBACK | Cannot use transaction control |

