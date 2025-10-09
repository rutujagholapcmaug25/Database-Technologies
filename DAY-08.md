# Views and Indexes

## 1. MySQL Views

### Definition
- A **view** is a virtual table derived from one or more base tables.
- Does **not store data** physically; shows current data from underlying tables.
- Useful for simplifying queries, improving security, and providing abstraction.

### Key Points
- Behaves like a real table with rows and columns.
- Any changes in underlying tables are reflected automatically.
- Can be used to restrict access to specific rows/columns.

### Creating a View
```sql
CREATE [OR REPLACE] VIEW view_name AS
SELECT column1, column2
FROM table_name
[WHERE condition];
```

**Example:**
```sql
CREATE VIEW trainer AS
SELECT course_name, trainer
FROM courses;

SELECT * FROM trainer;
```

### Modifying a View
```sql
ALTER VIEW view_name AS
SELECT column1, column2
FROM table_name
[WHERE condition];
```

### Updating Data through Views
- Allowed if:
  - All NOT NULL columns are included.
  - Based on a single table without complex joins or aggregates.
- INSERT, UPDATE, DELETE operations affect the underlying tables.

### Advantages
1. **Data Security:** Show only intended data.
2. **Simplification:** Store complex queries for easy retrieval.
3. **Consistency:** Multiple users see same results.
4. **Abstraction:** Hides underlying schema complexity.

---

## 2. MySQL Indexes

### Definition
- An **index** is a data structure (B-tree or hash) that speeds up data retrieval.
- Helps in `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`.
- Trade-off: Faster reads, slower writes (`INSERT`, `UPDATE`, `DELETE`).

### Syntax
```sql
-- Create index
CREATE INDEX index_name ON table_name(column_name);

-- Create unique index
CREATE UNIQUE INDEX index_name ON table_name(column_name);

-- Drop index
DROP INDEX index_name ON table_name;

-- Show indexes
SHOW INDEX FROM table_name;
```

### Index Types
| Type           | Description |
|----------------|-------------|
| PRIMARY INDEX  | Unique, not null, auto-created for primary key |
| UNIQUE INDEX   | Ensures column values are unique |
| INDEX (Normal) | Speeds up searches; allows duplicates |
| FULLTEXT       | Full-text search on text fields |
| SPATIAL        | Spatial / geometry data |
| COMPOSITE      | Index on multiple columns |
| DESCENDING     | MySQL 8+, stores index in reverse order |

### Composite Index Example
```sql
CREATE INDEX idx_name_city ON customers(name, city);

SELECT * FROM customers WHERE name='Ravi' AND city='Pune';
```
**Leftmost Prefix Rule:** For `(A,B,C)` → works for `(A)`, `(A,B)`, `(A,B,C)` but not `(B,C)`.

### USE INDEX Hint
- Instructs optimizer to prefer certain indexes.
```sql
SELECT select_list
FROM table_name USE INDEX(index_list)
WHERE condition;
```

**Example:**
```sql
SELECT * FROM customers USE INDEX(idx_name_fl, idx_name_lf)
WHERE contactFirstName LIKE 'A%' OR contactLastName LIKE 'A%';
```
