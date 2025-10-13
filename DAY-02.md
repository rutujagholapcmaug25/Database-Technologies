# MySQL

## 🔹 Definition
**MySQL** is the world’s most popular **open-source RDBMS**, developed and supported by **Oracle Corporation**.  
It stores data in **tables (rows & columns)** and uses **SQL** for data manipulation.

📍 **Official Website:** [https://www.mysql.com/](https://www.mysql.com)

---

## Key Features

| Feature | Description | Example / Use |
|----------|--------------|----------------|
| **Speed & Performance** | Optimized for read-heavy and transactional systems | Web apps, analytics |
| **ACID Compliance** | Ensures reliable transactions | Banking, billing |
| **Complex Queries** | Supports JOINs, subqueries, grouping | Reporting systems |
| **Scalability** | Handles large datasets; supports replication & sharding | Enterprise databases |
| **Replication** | Master-slave, multi-master, cluster | High availability systems |
| **Security** | SSL, encryption, access control | Secure applications |
| **Cross-Platform** | Works on Windows, Linux, macOS | Universal support |
| **Open Source** | Free under GPL; customizable | Community-driven |
| **Community Support** | Huge user base, docs, and tools | Developer-friendly |

---

## MySQL Architecture

| Component | Description |
|------------|--------------|
| **Client–Server Model** | Client sends SQL queries → Server processes & returns results |
| **Storage Engines** | Pluggable engines (InnoDB – default, MyISAM, Memory) |
| **InnoDB** | ACID compliant, supports FK & transactions |
| **MyISAM** | Fast, read-heavy, no transaction support |

---

## Core Components

| Component | Purpose |
|------------|----------|
| **MySQL Server** | Core engine that processes SQL and stores data |
| **MySQL Workbench** | GUI for database design & queries |
| **MySQL Client Tools** | CLI utilities (`mysql`, `mysqldump`, `mysqladmin`) |
| **MySQL Connectors** | APIs for Python, Java, C++, etc. |

---

## Common Data Types (Cheat Sheet)

| Category | Data Type | Example | Notes |
|-----------|------------|----------|--------|
| **Numeric** | `INT`, `BIGINT`, `DECIMAL`, `FLOAT` | `price DECIMAL(8,2)` | Use DECIMAL for money |
| **String** | `CHAR`, `VARCHAR`, `TEXT`, `ENUM`, `SET` | `name VARCHAR(50)` | VARCHAR most common |
| **Date/Time** | `DATE`, `DATETIME`, `TIMESTAMP` | `'2025-10-07 14:30:00'` | Use TIMESTAMP for auto updates |
| **Boolean** | `TINYINT(1)` | `is_active TINYINT(1)` | 0 = false, 1 = true |
| **JSON** | `JSON` | `{"id":1,"qty":2}` | For flexible data |

---

- Use **`INT`** for IDs; **`BIGINT`** for very large ranges.  
- Use **`DECIMAL`** for financial data (avoid `FLOAT`).  
- Use **`VARCHAR`** for variable text; **`CHAR`** for fixed length.  
- Use **`TIMESTAMP`** for created/updated fields.  
- Use **`ENUM` / `SET`** for controlled values.  
- Always choose **smallest data type** for better performance.  

---

## Real-Life Applications

| Domain | Use Case |
|---------|-----------|
| **Banking** | Transaction records, account management |
| **E-Commerce** | Product catalogs, user accounts, orders |
| **Education** | Student, course, and results data |
| **Healthcare** | Patient and appointment systems |
| **Web Apps** | Blogs, social media, CMS, analytics |

---

## 1. String Literals

```sql
mysql> select "Hello", 'Hello';
+-------+-------+
| Hello | Hello |
+-------+-------+
| Hello | Hello |
+-------+-------+
1 row in set (0.01 sec)
```

**Both single (`'`) and double (`"`) quotes can represent strings.**

---

## 2. Version & Current Database

```sql
mysql> select version(), database();
+-----------+------------+
| version() | database() |
+-----------+------------+
| 8.0.40    | NULL       |
+-----------+------------+
1 row in set (0.01 sec)
```

**`version()` shows MySQL version, `database()` returns NULL if no DB is selected.**

---

## 3. Aliases

```sql
mysql> select "Hello" col1, 'Hello' as col2;
+-------+-------+
| col1  | col2  |
+-------+-------+
| Hello | Hello |
+-------+-------+
1 row in set (0.01 sec)

mysql> select version() as ver, database() as db;
+--------+------+
| ver    | db   |
+--------+------+
| 8.0.40 | NULL |
+--------+------+
1 row in set (0.00 sec)
```

**`AS` renames columns.**

---

## 4. Mixing Quotes

```sql
mysql> select "Hello", 'Hello', '"Hello"', "'Hello'";
+-------+-------+---------+---------+
| Hello | Hello | "Hello" | 'Hello' |
+-------+-------+---------+---------+
| Hello | Hello | "Hello" | 'Hello' |
+-------+-------+---------+---------+
1 row in set (0.00 sec)
```

**Escaping quotes with `\`.**

---

## 5. Strings with Newline

```sql
mysql> select 'This\n is\n Shivjanmabhoomi.';
+----------------------------+
| This
 is
 Shivjanmabhoomi. |
+----------------------------+
| This
 is
 Shivjanmabhoomi. |
+----------------------------+
1 row in set (0.00 sec)
```

**`\n` creates line breaks.**

---

## 6. Arithmetic

```sql
mysql> select 2+3;
+-----+
| 2+3 |
+-----+
|   5 |
+-----+
1 row in set (0.02 sec)

mysql> select 2+3 as addition;
+----------+
| addition |
+----------+
|        5 |
+----------+
1 row in set (0.00 sec)
```

---

## 7. String to Number Conversion

```sql
mysql> select  "4"+ " 5";
+-----------+
| "4"+ " 5" |
+-----------+
|         9 |
+-----------+
1 row in set (0.01 sec)

mysql> select  '4'+ '5';
+----------+
| '4'+ '5' |
+----------+
|        9 |
+----------+
1 row in set (0.00 sec)
```

**Strings with numbers are auto-converted.**

---

## 8. Date Literals

```sql
mysql> SELECT DATE'20030601';
+----------------+
| DATE'20030601' |
+----------------+
| 2003-06-01     |
+----------------+
1 row in set (0.01 sec)

mysql> SELECT DATE'20031301';
ERROR 1525 (HY000): Incorrect DATE value: '20031301'

mysql> SELECT DATE'01062003';
ERROR 1525 (HY000): Incorrect DATE value: '01062003'

mysql> SELECT DATE'251201';
+--------------+
| DATE'251201' |
+--------------+
| 2025-12-01   |
+--------------+
1 row in set (0.00 sec)
```

**MySQL checks valid date formats.**

---

## 9. Boolean Values

```sql
mysql> SELECT TRUE;
+------+
| TRUE |
+------+
|    1 |
+------+
1 row in set (0.01 sec)

mysql> SELECT FALSE;
+-------+
| FALSE |
+-------+
|     0 |
+-------+
1 row in set (0.00 sec)
```

---

## 10. IF() Function

```sql
mysql> SELECT IF(TRUE=1, "TRUE", "FALSE");
+-----------------------------+
| IF(TRUE=1, "TRUE", "FALSE") |
+-----------------------------+
| TRUE                        |
+-----------------------------+
1 row in set (0.02 sec)

mysql> SELECT IF(TRUE=2, "TRUE", "FALSE");
+-----------------------------+
| IF(TRUE=2, "TRUE", "FALSE") |
+-----------------------------+
| FALSE                       |
+-----------------------------+
1 row in set (0.00 sec)

mysql> SELECT IF(FALSE=2, "TRUE", "FALSE");
+------------------------------+
| IF(FALSE=2, "TRUE", "FALSE") |
+------------------------------+
| FALSE                        |
+------------------------------+
1 row in set (0.00 sec)

mysql> SELECT IF(FALSE=0, "TRUE", "FALSE");
+------------------------------+
| IF(FALSE=0, "TRUE", "FALSE") |
+------------------------------+
| TRUE                         |
+------------------------------+
1 row in set (0.00 sec)
```

---

## 11. Database Commands

```sql
mysql> CREATE DATABASE DACAUG25;
mysql> SHOW DATABASES;
mysql> USE DACAUG25;
mysql> SELECT DATABASE();
```

**Created and switched to `dacaug25` database.**

---

## 12. CRUD Operations on STUDENTS Table

### 12.1 Create Table

**Concept:** Create the `STUDENTS` table with ID, NAME, MOB columns.

**Input:**

```sql
mysql> CREATE TABLE STUDENTS(ID INT, NAME VARCHAR(50), MOB VARCHAR(50));
mysql> SHOW TABLES;
```

**Output:**

```text
+--------------------+
| Tables_in_dacaug25 |
+--------------------+
| students           |
+--------------------+
1 row in set (0.04 sec)
```

### 12.2 Insert Data

**Concept:** Insert rows into `STUDENTS`.

**Input:**

```sql
mysql> INSERT INTO STUDENTS VALUES(1, "RUTUJA", 8830482634);
mysql> INSERT INTO STUDENTS(ID, MOB) VALUES(2, 3377838833);
mysql> INSERT INTO STUDENTS VALUES(3, "ANUJA", 7676889965),(4, "vrunda", 4576983432);
mysql> SELECT * FROM STUDENTS;
```

**Output:**

```text
+------+--------+------------+
| ID   | NAME   | MOB        |
+------+--------+------------+
|    1 | RUTUJA | 8830482634 |
|    2 | NULL   | 3377838833 |
|    3 | ANUJA  | 7676889965 |
|    4 | Vrunda| 4576983432 |
+------+--------+------------+
4 rows in set (0.00 sec)
```

### 12.3 Read Data

**Concept:** Retrieve rows from the table.

**Input:**

```sql
mysql> SELECT NAME FROM STUDENTS;
mysql> SELECT ID, NAME FROM STUDENTS;
mysql> SELECT ID, MOB, NAME FROM STUDENTS;
mysql> SELECT * FROM STUDENTS WHERE ID = 4;
```

**Output:**

```text
+--------+
| NAME   |
+--------+
| RUTUJA |
| ANUJA  |
| Vrunda|
+--------+
3 rows in set (0.00 sec)

+------+--------+
| ID   | NAME   |
+------+--------+
|    1 | RUTUJA |
|    2 | NULL   |
|    3 | ANUJA  |
|    4 | Vrunda|
+------+--------+
4 rows in set (0.00 sec)

+------+------------+--------+
| ID   | MOB        | NAME   |
+------+------------+--------+
|    1 | 8830482634 | RUTUJA |
|    2 | 3377838833 | NULL   |
|    3 | 7676889965 | ANUJA  |
|    4 | 4576983432 | Vrunda|
+------+------------+--------+
4 rows in set (0.00 sec)

+------+--------+------------+
| ID   | NAME   | MOB        |
+------+--------+------------+
|    4 | Vrunda| 4576983432 |
+------+--------+------------+
1 row in set (0.00 sec)
```

### 12.4 Update Data

**Concept:** Modify existing data.

**Input:**

```sql
mysql> UPDATE STUDENTS SET MOB = 7538493940 WHERE NAME = "vrunda";
mysql> SELECT * FROM STUDENTS WHERE NAME = "vrunda";
```

**Output:**

```text
Query OK, 1 row affected (0.03 sec)
Rows matched: 1  Changed: 1  Warnings: 0

+------+--------+------------+
| ID   | NAME   | MOB        |
+------+--------+------------+
|    4 | Vrunda| 7538493940 |
+------+--------+------------+
1 row in set (0.00 sec)
```

## 12.5 Delete Data

**Concept:** Remove specific rows from the table.

**Input & Output:**

```sql
mysql> DELETE FROM STUDENTS WHERE ID = 2;
Query OK, 1 row affected (0.02 sec)

mysql> SELECT * FROM STUDENTS;
+------+--------+------------+
| ID   | NAME   | MOB        |
+------+--------+------------+
|    1 | RUTUJA | 8830482634 |
|    3 | ANUJA  | 7676889965 |
|    4 | Vrunda| 7538493940 |
+------+--------+------------+
3 rows in set (0.00 sec)
```

## 12.6 Truncate Table

**Concept:** Remove all rows but keep the table structure intact.

**Input & Output:**

```sql
mysql> TRUNCATE TABLE STUDENTS;
Query OK, 0 rows affected (0.20 sec)

mysql> SELECT * FROM STUDENTS;
Empty set (0.01 sec)
```

## 12.7 Drop Table

**Concept:** Delete the table completely from the database.

**Input & Output:**

```sql
mysql> DROP TABLE STUDENTS;
Query OK, 0 rows affected (0.05 sec)

mysql> SHOW TABLES;
Empty set (0.00 sec)
```
