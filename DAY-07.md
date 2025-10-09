# MySQL UNION, FULL OUTER JOIN, Execution Order & Subqueries

---

## **1. UNION and UNION ALL**

### **UNION**
- Combines results from two or more `SELECT` queries into a single result set.
- Removes duplicate rows by default (like a mathematical set union).
- Each `SELECT` must have the same number of columns and compatible data types.
- Syntax:
```sql
SELECT column_list FROM table1
UNION
SELECT column_list FROM table2;
```

### **UNION ALL**
- Combines multiple `SELECT` queries.
- Keeps duplicates (does not remove them).
- Faster than `UNION` because it skips duplicate removal.
- Syntax:
```sql
SELECT column_list FROM table1
UNION ALL
SELECT column_list FROM table2;
```

---

## **2. Full Outer Join (Emulation in MySQL)**

- MySQL (up to 8.x) doesn’t natively support `FULL OUTER JOIN`.
- Can be simulated using `UNION` of `LEFT JOIN` and `RIGHT JOIN`.

```sql
SELECT c.customer_id, c.name, o.order_id, o.amount, o.status
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
UNION
SELECT c.customer_id, c.name, o.order_id, o.amount, o.status
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id;
```

---

## **3. SQL Execution Order vs Writing Order**

| Writing Order | Execution Order |
|---------------|----------------|
| SELECT        | FROM            |
| FROM          | WHERE           |
| WHERE         | GROUP BY        |
| GROUP BY      | HAVING          |
| HAVING        | SELECT          |
| ORDER BY      | ORDER BY        |

**Example:**
```sql
SELECT cgpa*10 AS marks FROM student WHERE marks > 70; -- Error
SELECT cgpa*10 AS marks FROM student HAVING marks > 70; -- Works
```

---

## **4. MySQL Subqueries**

A **subquery** is a `SELECT` statement nested inside another statement.

```sql
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
```
- The outer query is `SELECT * FROM t1`.
- The subquery `(SELECT column1 FROM t2)` is executed first.
- Subqueries must be enclosed in parentheses.

### **Advantages**
- Isolate query logic into parts.
- Alternative to complex joins/unions.
- Often more readable.

### **Subquery Types**

1. **Scalar Subquery**
   - Returns a single value.
   ```sql
   SELECT * FROM t1 WHERE column1 = (SELECT MAX(column2) FROM t2);
   ```

2. **Comparison Using Subqueries**
   - Works with `=, >, <, >=, <=, <>, !=, <=>`.
   ```sql
   SELECT * FROM t1 WHERE column1 = (SELECT MAX(column2) FROM t2);
   ```

3. **ANY, IN, SOME**
   - Compare operand with any or all values from subquery.
   ```sql
   SELECT s1 FROM t1 WHERE s1 > ANY (SELECT s1 FROM t2);
   ```

4. **ALL**
   - Returns TRUE if comparison holds for all values.
   ```sql
   SELECT s1 FROM t1 WHERE s1 > ALL (SELECT s1 FROM t2);
   ```

5. **Row Subqueries**
   - Return single row with multiple columns.
   ```sql
   SELECT * FROM t1 WHERE (col1, col2) = (SELECT col3, col4 FROM t2 WHERE id=10);
   ```

6. **EXISTS / NOT EXISTS**
   - `EXISTS` returns TRUE if subquery has rows.
   - `NOT EXISTS` returns TRUE if subquery returns no rows.
   ```sql
   SELECT column1 FROM t1 WHERE EXISTS (SELECT * FROM t2);
   ```

7. **Correlated Subqueries**
   - Refers to a column from the outer query.
   - Subquery is evaluated for each row of outer query.
   ```sql
   SELECT * FROM t1
   WHERE column1 = ANY (SELECT column1 FROM t2 WHERE t2.column2 = t1.column2);
   ```

**Rules for Correlated Subqueries**
- Correlated column only in `WHERE`.
- Cannot appear in `SELECT, JOIN, ORDER BY, GROUP BY, HAVING`.
- Cannot be in derived tables, aggregate, or window functions.

---

## **5. Operator Summary in Subqueries**

| Operator | Meaning |
|----------|---------|
| IN       | Equal to any value in a list |
| ANY      | Compare with any value returned |
| ALL      | Compare with all values returned |
| EXISTS   | TRUE if any row exists in subquery |

---

