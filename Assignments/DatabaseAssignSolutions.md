# Library Management System - SQL Questions & Answers

## Schema Design & Table Creation (DDL & DML Commands)

### 1. Define a schema for a Library Management System with the following entities:
- Books
- Authors
- Members
- Borrow_Records

**Answer:**
```sql
CREATE DATABASE LMS;
USE LMS;
```

---

### 2. Write the SQL command to create a table Authors with the following fields:
- author_id (Primary Key, INT)
- name (VARCHAR(100))
- country (VARCHAR(50))

**Answer:**
```sql
CREATE TABLE author(
    author_id INT PRIMARY KEY, 
    author_name VARCHAR(50), 
    country VARCHAR(50)
);
```

---

### 3. Write the SQL command to create a table Books with the following fields:
- book_id (Primary Key, INT)
- title (VARCHAR(150))
- author_id (Foreign Key referencing Authors)
- published_year (YEAR)
- available_copies (INT)

**Answer:**
```sql
CREATE TABLE books(
    book_id INT PRIMARY KEY, 
    author_id INT, 
    published_year YEAR, 
    available_copies INT, 
    FOREIGN KEY(author_id) REFERENCES author(author_id)
);

-- Add title column
ALTER TABLE books ADD COLUMN title VARCHAR(150);

-- Modify title column to VARCHAR(50)
ALTER TABLE books MODIFY title VARCHAR(50);
```

---

### 4. Write the SQL command to create a table Members with:
- member_id (Primary Key, INT)
- name (VARCHAR(100))
- email (VARCHAR(100), unique)
- phone (VARCHAR(15))

**Answer:**
```sql
CREATE TABLE members(
    member_id INT PRIMARY KEY, 
    member_name VARCHAR(50), 
    member_email VARCHAR(50) UNIQUE, 
    phone VARCHAR(50)
);
```

---

### 5. Write the SQL command to create a table Borrow_Records with:
- record_id (Primary Key, INT)
- member_id (Foreign Key referencing Members)
- book_id (Foreign Key referencing Books)
- borrow_date (DATE)
- return_date (DATE)

**Answer:**
```sql
CREATE TABLE Borrow_Records(
    record_id INT PRIMARY KEY, 
    member_id INT,
    book_id INT, 
    borrow_date DATE, 
    return_date DATE, 
    FOREIGN KEY(member_id) REFERENCES members(member_id), 
    FOREIGN KEY(book_id) REFERENCES books(book_id)
);
```

---

### 6. Modify the Books table to add a column genre of type VARCHAR(50).

**Answer:**
```sql
ALTER TABLE books ADD COLUMN genre VARCHAR(50);
```

---

### 7. Write the SQL command to drop the Borrow_Records table.

**Answer:**
```sql
DROP TABLE Borrow_Records;
```

---

### 8. Insert 3 records into the Authors table.

**Answer:**
```sql
INSERT INTO author(author_id, author_name, country) 
VALUES
    (101, 'abc', 'India'), 
    (102, 'xyz', 'India'),
    (103, 'lmn', 'US');
```

---

### 9. Insert 5 books into the Books table.

**Answer:**
```sql
INSERT INTO books(book_id, author_id, published_year, available_copies, title) 
VALUES
    (1, 101, 2012, 3, 'COS'),
    (2, 102, 2013, 4, 'ADS'),
    (3, 102, 2023, 1, 'OOPJ'),
    (4, 103, 2020, 3, 'DBT'),
    (5, 101, 2001, 0, 'Java');
```

---

### 10. Insert 3 members into the Members table.

**Answer:**
```sql
INSERT INTO members(member_id, member_name, member_email, phone) 
VALUES
    (111, 'Anuja', 'anuja9@gmail.com', '8756436798'),
    (112, 'Rutuja', 'rutuja@gmail.com', '8789675423'),
    (113, 'Sanika', 'sanika@gmail.com', '8756879789');
```

---

### 11. Insert 4 borrow records into the Borrow_Records table.

**Answer:**
```sql
INSERT INTO borrow_records (record_id, member_id, book_id, borrow_date, return_date) 
VALUES 
    (1, 111, 1, '2025-09-01', '2025-09-10'), 
    (2, 112, 2, '2025-09-05', '2025-09-12'), 
    (3, 113, 3, '2025-09-07', '2025-09-15'), 
    (4, 111, 4, '2025-09-08', '2025-09-18');
```

---

### 12. Write an SQL query to select all books where published_year is after 2015.

**Answer:**
```sql
SELECT * FROM books WHERE published_year > 2015;
```

**Output:**
```
+---------+-----------+----------------+------------------+-------+
| book_id | author_id | published_year | available_copies | title |
+---------+-----------+----------------+------------------+-------+
|       3 |       102 |           2023 |                1 | OOPJ  |
|       4 |       103 |           2020 |                3 | DBT   |
+---------+-----------+----------------+------------------+-------+
```

---

### 13. Write a SQL query to create a foreign key & primary key relationship between two tables.

**Answer:**
```sql
ALTER TABLE books 
ADD CONSTRAINT fk_author 
FOREIGN KEY(author_id) 
REFERENCES author(author_id);
```

---

### 14. Write an SQL query to find all members who have borrowed the book with title 'Database Systems'.

**Answer:**
```sql
SELECT m.member_id, m.member_name, m.member_email, m.phone 
FROM members m
JOIN borrow_records AS br ON m.member_id = br.member_id
JOIN books AS b ON br.book_id = b.book_id
WHERE b.title = "DBT";
```

**Output:**
```
+-----------+-------------+------------------+------------+
| member_id | member_name | member_email     | phone      |
+-----------+-------------+------------------+------------+
|       111 | Anuja       | anuja9@gmail.com | 8756436798 |
+-----------+-------------+------------------+------------+
```

---

### 15. Update the available_copies column of a specific book (choose any book) by reducing it by 1 after it is borrowed.

**Answer:**
```sql
UPDATE books
SET available_copies = available_copies - 1
WHERE book_id = 1;
```

---

### 16. Delete a record from Members where member_id = 3.

**Answer:**
```sql
DELETE FROM members
WHERE member_id = 3;
```

---

### 17. Update a Book name record from Book table with id = 1.

**Answer:**
```sql
UPDATE books
SET title = "New Book"
WHERE book_id = 1;
```

---

### 18. Write an SQL query to list all books along with their authors' names.

**Answer:**
```sql
SELECT b.book_id, b.title, a.author_name, a.country
FROM books b
JOIN author a ON b.author_id = a.author_id;
```

**Output:**
```
+---------+----------+-------------+---------+
| book_id | title    | author_name | country |
+---------+----------+-------------+---------+
|       1 | New Book | abc         | India   |
|       5 | Java     | abc         | India   |
|       2 | ADS      | xyz         | India   |
|       3 | OOPJ     | xyz         | India   |
|       4 | DBT      | lmn         | US      |
+---------+----------+-------------+---------+
```

---

### 19. Write an SQL query to delete all books from the Books table where the published_year is before 2000.

**Answer:**
```sql
DELETE FROM books
WHERE published_year < 2000;
```

---

### 20. Write an SQL query to find all books that are never borrowed (i.e., no records in Borrow_Records).

**Answer:**
```sql
SELECT b.book_id, b.title
FROM books b
LEFT JOIN borrow_records br ON b.book_id = br.book_id
WHERE br.book_id IS NULL;
```

**Output:**
```
+---------+-------+
| book_id | title |
+---------+-------+
|       5 | Java  |
+---------+-------+
```
