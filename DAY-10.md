# PL/SQL 

## 1. IF Function
- **Definition:** Returns a value based on a condition. Also called IF-ELSE or IF-THEN-ELSE function.
- **Syntax:**
```sql
IF(expression1, expression2, expression3)
```
- **Example:**
```sql
SELECT IF(200 > 350, 'YES', 'NO') AS result;
```
- **Explanation:** Returns `expression2` if `expression1` is true; otherwise returns `expression3`.

---

## 2. IF-THEN Statement
- **Definition:** Executes SQL statements if a condition is true.
- **Syntax:**
```sql
IF condition THEN
    statements;
END IF;
```
- **Example:**
```sql
DELIMITER $$
CREATE PROCEDURE myResult(original_rate FLOAT, OUT discount_rate FLOAT)
NO SQL
BEGIN
    IF (original_rate > 200) THEN
        SET discount_rate = original_rate * 0.5;
    ELSE
        SET discount_rate = original_rate;
    END IF;
    SELECT discount_rate;
END$$
DELIMITER ;

-- Call procedure
SET @p = 150;
SET @dp = 180;
CALL myResult(@p, @dp);
```

---

## 3. IF-THEN-ELSEIF-ELSE Statement
- **Definition:** Executes statements based on multiple conditions.
- **Syntax:**
```sql
IF condition THEN
    statements;
ELSEIF elseif_condition THEN
    elseif_statements;
ELSE
    else_statements;
END IF;
```

---

## 4. CASE Statement
- **Definition:** Validates multiple conditions and returns a result when the first condition is true.
- **Syntax (Simple CASE):**
```sql
CASE value
    WHEN compare_value THEN result
    [WHEN compare_value THEN result ...]
    [ELSE result]
END
```
- **Syntax (Searched CASE):**
```sql
CASE
    WHEN condition THEN result
    [WHEN condition THEN result ...]
    [ELSE result]
END
```
- **Example:**
```sql
SELECT id, name, marks, class,
CASE class
    WHEN 'TY' THEN '3rd Year'
    WHEN 'BE' THEN 'Final Year'
    WHEN 'SY' THEN '2nd Year'
    ELSE 'NONE'
END AS year
FROM student;
```

---

## 5. Loops
### 5.1 LOOP
- **Definition:** Executes a set of statements repeatedly.
- **Syntax:**
```sql
label: LOOP
    statement_list
END LOOP label;
```
- **Termination:** `LEAVE` or `RETURN`

### 5.2 WHILE Loop
- **Definition:** Executes statements while a condition is true.
- **Syntax:**
```sql
label: WHILE search_condition DO
    statement_list
END WHILE label;
```

### 5.3 REPEAT Loop
- **Definition:** Repeats statements until a condition becomes true.
- **Syntax:**
```sql
label: REPEAT
    statement_list
UNTIL search_condition
END REPEAT label;
```

### 5.4 LEAVE Statement
- **Definition:** Exits LOOP, REPEAT, WHILE, or BEGIN...END blocks.
- **Syntax:**
```sql
LEAVE label;
```

### 5.5 ITERATE Statement
- **Definition:** Restarts the LOOP, REPEAT, or WHILE statement.
- **Syntax:**
```sql
ITERATE label;
```

---

## 6. MySQL Stored Functions
- **Definition:** Performs a task and returns a single value.
- **Differences from Procedures:**
  - Can only have IN parameters
  - Returns a single value
  - Can be called within SQL statements
  - Cannot produce a result set
- **Syntax:**
```sql
DELIMITER $$
CREATE FUNCTION function_name(parameter_list)
RETURNS data_type
[NOT] {DETERMINISTIC | NO SQL | READS SQL DATA}
BEGIN
    -- SQL statements
    RETURN value;
END $$
DELIMITER ;
```
- **Example:**
```sql
DELIMITER $$
CREATE FUNCTION addNumbers(a INT, b INT)
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN a + b;
END $$
DELIMITER ;

-- Call function
SELECT addNumbers(10, 20);
```

---

## 7. MySQL Stored Procedures
- **Definition:** Executes a set of SQL statements grouped under a single name. May modify data or return result sets.
- **Benefits:** Reusability, Modularity, Security, Performance
- **Syntax:**
```sql
DELIMITER $$
CREATE PROCEDURE procedure_name(
    IN parameter1 datatype,
    OUT parameter2 datatype,
    INOUT parameter3 datatype
)
BEGIN
    -- SQL statements
END $$
DELIMITER ;
```
- **Parameters:**
  - **IN:** Input only, original value protected
  - **OUT:** Output only, value updated in calling program
  - **INOUT:** Both input and output
- **Example:**
```sql
DELIMITER $$
CREATE PROCEDURE calculateDiscount(IN original_rate FLOAT, OUT discount_rate FLOAT)
BEGIN
    IF original_rate > 200 THEN
        SET discount_rate = original_rate * 0.5;
    ELSE
        SET discount_rate = original_rate;
    END IF;
END $$
DELIMITER ;

-- Call procedure
SET @orig = 250;
SET @disc = 0;
CALL calculateDiscount(@orig, @disc);
SELECT @disc;
```

