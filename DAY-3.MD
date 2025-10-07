# MySQL Transaction Control & Constraints — Complete Interview Notes

---

## Transaction Control in MySQL

Transactions ensure data consistency — a group of SQL statements executed as a single unit (either all succeed or none).

### Key Commands

| Command | Description | Example / Notes |
|----------|--------------|----------------|
| **AUTOCOMMIT** | If enabled (`SET autocommit=1`), every SQL statement is automatically committed. | Default in MySQL. |
| **SET autocommit = 0** | Turns off autocommit; transactions must be committed manually. | Use in multi-step DML operations. |
| **START TRANSACTION / BEGIN** | Starts a new transaction. | `START TRANSACTION;` |
| **COMMIT** | Makes all changes permanent. | Confirms transaction changes. |
| **ROLLBACK** | Cancels all changes made in the current transaction. | Restores previous state. |
| **SAVEPOINT** | Creates a named point to roll back to. | `SAVEPOINT sp1;` |
| **ROLLBACK TO SAVEPOINT** | Rolls back only up to the savepoint. | `ROLLBACK TO sp1;` |

### Quick Notes

- When **autocommit = ON** → each SQL executes & commits automatically.  
- When **autocommit = OFF** → you must explicitly use `COMMIT` or `ROLLBACK`.  
- If session ends without `COMMIT`, MySQL automatically **ROLLBACKs**.  
- Both `COMMIT` and `ROLLBACK` **release locks** held during the transaction.  

---

## Example — Transaction Flow

```sql
CREATE TABLE customer (id INT, name VARCHAR(50));

-- Start transaction manually
SET autocommit = 0;
START TRANSACTION;

INSERT INTO customer VALUES (1, 'Amit');
SAVEPOINT sp1;

INSERT INTO customer VALUES (2, 'Riya');
ROLLBACK TO sp1;   -- Undo last insert only

COMMIT;            -- Make changes permanent


---

# MySQL Constraints

---

## MySQL Constraints

Constraints are rules applied on table columns to ensure **data integrity** and **validity**.

| Constraint        | Purpose                                         | Example                                   | Notes                               |
|------------------|-------------------------------------------------|-------------------------------------------|-------------------------------------|
| **NOT NULL**       | Prevents NULL values                           | `name VARCHAR(50) NOT NULL`               | Ensures mandatory fields.          |
| **UNIQUE**         | Ensures no duplicate values                    | `email VARCHAR(100) UNIQUE`               | Allows one NULL value.             |
| **PRIMARY KEY**    | Uniquely identifies each row                   | `student_id INT PRIMARY KEY`              | Auto indexed, NOT NULL.            |
| **FOREIGN KEY**    | Links two tables using PK of another          | `FOREIGN KEY (course_id) REFERENCES Courses(course_id)` | Maintains referential integrity. |
| **DEFAULT**        | Sets default value when none given             | `status VARCHAR(10) DEFAULT 'Active'`    | Saves null-checks in app code.     |
| **CHECK**          | Ensures condition on data values               | `CHECK (credits > 0)`                     | Available in MySQL 8.0+.          |
| **AUTO_INCREMENT** | Generates sequential numeric IDs automatically | `student_id INT AUTO_INCREMENT`          | Used with PK columns.              |

---

## Why Constraints Matter

- Enforce **data accuracy** automatically.  
- Ensure **relationships** (FK) and **uniqueness** (PK).  
- Reduce **application-side validation**.  
- Support **ACID principles** through consistency.

---

## Example — Constraints in Action

```sql
DROP DATABASE IF EXISTS demo_college;
CREATE DATABASE demo_college;
USE demo_college;

-- Students table
CREATE TABLE Students (
  student_id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  email VARCHAR(100) UNIQUE
);

-- Courses table
CREATE TABLE Courses (
  course_id INT AUTO_INCREMENT PRIMARY KEY,
  course_name VARCHAR(100) NOT NULL,
  credits INT CHECK (credits > 0)
);

-- Enrollments table (relationship)
CREATE TABLE Enrollments (
  enrollment_id INT AUTO_INCREMENT PRIMARY KEY,
  student_id INT,
  course_id INT,
  grade CHAR(2),
  FOREIGN KEY (student_id) REFERENCES Students(student_id),
  FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);

---

## Transactions

- Maintain **Atomicity** (all-or-nothing).  
- Use **SAVEPOINT** for partial rollbacks.  
- **COMMIT** confirms, **ROLLBACK** cancels.

## Constraints

- Enforce **Integrity & Accuracy**.  
- **PK + FK** define relationships.  
- Use **CHECK, DEFAULT, and AUTO_INCREMENT** effectively.

## Note

> Transactions ensure reliable changes in data, while Constraints ensure valid data.  
> Together they guarantee the **ACID properties** — Atomicity, Consistency, Isolation, and Durability.
