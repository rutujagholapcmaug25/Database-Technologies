# MySQL Revision Guide

## Database and Table Creation

```sql
CREATE DATABASE revision;
USE revision;

CREATE TABLE customers (
    Id INT,
    Name VARCHAR(20),
    Email VARCHAR(25),
    City VARCHAR(20)
);
```

```sql
DESC customers;
```
```
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| Id    | int         | YES  |     | NULL    |       |
| Name  | varchar(20) | YES  |     | NULL    |       |
| Email | varchar(25) | YES  |     | NULL    |       |
| City  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```

```sql
RENAME TABLE customers TO cust1;
```

## Transaction Management

```sql
SELECT @@autocommit;
```
```
+--------------+
| @@autocommit |
+--------------+
|            0 |
+--------------+
```

```sql
SET autocommit = 0;
START TRANSACTION;

INSERT INTO customers VALUES(1, "Rutuja", "rutuja@gmail.com", "Pune");
INSERT INTO customers(name, email, city) VALUES("Anuja", "anuja@gmail.com", "Pune");

SELECT * FROM customers;
```
```
+------+--------+------------------+------+
| Id   | Name   | Email            | City |
+------+--------+------------------+------+
|    1 | Rutuja | rutuja@gmail.com | Pune |
| NULL | Anuja  | anuja@gmail.com  | Pune |
+------+--------+------------------+------+
```

```sql
ROLLBACK;
SELECT * FROM customers;
```
```
Empty set
```

```sql
-- With autocommit = 1
SET autocommit = 1;
INSERT INTO customers VALUES(1, "Rutuja", "rutuja@gmail.com", "Pune");
INSERT INTO customers(name, email, city) VALUES("Anuja", "anuja@gmail.com", "Pune");

ROLLBACK;
SELECT * FROM customers;
```
```
+------+--------+------------------+------+
| Id   | Name   | Email            | City |
+------+--------+------------------+------+
|    1 | Rutuja | rutuja@gmail.com | Pune |
| NULL | Anuja  | anuja@gmail.com  | Pune |
+------+--------+------------------+------+
-- ROLLBACK has no effect when autocommit = 1
```

### Savepoints

```sql
SET autocommit = 0;
INSERT INTO customers(id, name, email, city) VALUES
(3, "Ayush", "ayush@gmail.com", "Pune"),
(4, "Vrunda", "vrunda@gmail.com", "Junnar"),
(5, "Sanika", "sanika@gmail.com", "Junnar");

SAVEPOINT s1;

UPDATE customers SET id = 10 WHERE id = 4;
SELECT * FROM customers;
```
```
+------+--------+------------------+--------+
| Id   | Name   | Email            | City   |
+------+--------+------------------+--------+
|    1 | Rutuja | rutuja@gmail.com | Pune   |
| NULL | Anuja  | anuja@gmail.com  | Pune   |
|    3 | Ayush  | ayush@gmail.com  | Pune   |
|   10 | Vrunda | vrunda@gmail.com | Junnar |
|    5 | Sanika | sanika@gmail.com | Junnar |
+------+--------+------------------+--------+
```

```sql
ROLLBACK TO s1;
SELECT * FROM customers;
```
```
+------+--------+------------------+--------+
| Id   | Name   | Email            | City   |
+------+--------+------------------+--------+
|    1 | Rutuja | rutuja@gmail.com | Pune   |
| NULL | Anuja  | anuja@gmail.com  | Pune   |
|    3 | Ayush  | ayush@gmail.com  | Pune   |
|    4 | Vrunda | vrunda@gmail.com | Junnar |
|    5 | Sanika | sanika@gmail.com | Junnar |
+------+--------+------------------+--------+
```

### NULL Handling

```sql
-- ❌ Wrong
UPDATE customers SET id = 2 WHERE id = NULL;
-- Returns: 0 rows affected

-- ✅ Correct
UPDATE customers SET id = 2 WHERE id IS NULL;
-- Returns: 1 row affected
```

## Constraints

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name        VARCHAR(80) NOT NULL,
    email       VARCHAR(120) UNIQUE,
    city        VARCHAR(80)
);

CREATE TABLE orders (
    order_id     INT PRIMARY KEY AUTO_INCREMENT,
    customer_id  INT NOT NULL,
    order_date   DATE NOT NULL,
    amount       DECIMAL(10,2) NOT NULL CHECK (amount >= 0),
    status       ENUM('NEW','PAID','CANCELLED') DEFAULT 'NEW',
    CONSTRAINT fk_orders_customer 
        FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

INSERT INTO customers (name, email, city) VALUES
('Amit Sharma','amit@shop.com','Mumbai'),
('Bhavna Iyer','bhavna@shop.com','Pune'),
('Chetan Rao','chetan@shop.com','Delhi'),
('Divya Menon','divya@shop.com','Mumbai'),
('Esha Khan','esha@shop.com','Bengaluru');

INSERT INTO orders (customer_id, order_date, amount, status) VALUES
(1,'2025-09-01',1200.00,'PAID'),
(1,'2025-09-10', 750.00,'PAID'),
(2,'2025-09-11',  99.00,'NEW'),
(3,'2025-09-12', 560.00,'CANCELLED'),
(3,'2025-09-20',2100.00,'PAID'),
(4,'2025-09-25', 300.00,'PAID'),
(5,'2025-09-30', 890.00,'NEW'),
(2,'2025-10-01', 450.00,'PAID');
```

```sql
SELECT * FROM customers;
```
```
+-------------+-------------+-----------------+-----------+
| customer_id | name        | email           | city      |
+-------------+-------------+-----------------+-----------+
|           1 | Amit Sharma | amit@shop.com   | Mumbai    |
|           2 | Bhavna Iyer | bhavna@shop.com | Pune      |
|           3 | Chetan Rao  | chetan@shop.com | Delhi     |
|           4 | Divya Menon | divya@shop.com  | Mumbai    |
|           5 | Esha Khan   | esha@shop.com   | Bengaluru |
+-------------+-------------+-----------------+-----------+
```

```sql
SELECT * FROM orders;
```
```
+----------+-------------+------------+---------+-----------+
| order_id | customer_id | order_date | amount  | status    |
+----------+-------------+------------+---------+-----------+
|        1 |           1 | 2025-09-01 | 1200.00 | PAID      |
|        2 |           1 | 2025-09-10 |  750.00 | PAID      |
|        3 |           2 | 2025-09-11 |   99.00 | NEW       |
|        4 |           3 | 2025-09-12 |  560.00 | CANCELLED |
|        5 |           3 | 2025-09-20 | 2100.00 | PAID      |
|        6 |           4 | 2025-09-25 |  300.00 | PAID      |
|        7 |           5 | 2025-09-30 |  890.00 | NEW       |
|        8 |           2 | 2025-10-01 |  450.00 | PAID      |
+----------+-------------+------------+---------+-----------+
```

## Querying

```sql
SELECT customer_id, name, city
FROM customers
WHERE city IN ('Mumbai', 'Pune')
ORDER BY name ASC
LIMIT 3;
```
```
+-------------+-------------+--------+
| customer_id | name        | city   |
+-------------+-------------+--------+
|           1 | Amit Sharma | Mumbai |
|           2 | Bhavna Iyer | Pune   |
|           4 | Divya Menon | Mumbai |
+-------------+-------------+--------+
```

```sql
-- AND (both conditions must be true)
SELECT customer_id, name, city
FROM customers
WHERE city='Mumbai' AND city='Pune';
```
```
Empty set
```

```sql
-- OR (any condition can be true)
SELECT customer_id, name, city
FROM customers
WHERE city='Mumbai' OR city='Pune';
```
```
+-------------+-------------+--------+
| customer_id | name        | city   |
+-------------+-------------+--------+
|           1 | Amit Sharma | Mumbai |
|           2 | Bhavna Iyer | Pune   |
|           4 | Divya Menon | Mumbai |
+-------------+-------------+--------+
```

## Pattern Matching (LIKE)

```sql
SELECT * FROM orders WHERE status LIKE "c%";
```
```
+----------+-------------+------------+--------+-----------+
| order_id | customer_id | order_date | amount | status    |
+----------+-------------+------------+--------+-----------+
|        4 |           3 | 2025-09-12 | 560.00 | CANCELLED |
+----------+-------------+------------+--------+-----------+
```

```sql
SELECT * FROM orders WHERE status LIKE "p%";
```
```
+----------+-------------+------------+---------+--------+
| order_id | customer_id | order_date | amount  | status |
+----------+-------------+------------+---------+--------+
|        1 |           1 | 2025-09-01 | 1200.00 | PAID   |
|        2 |           1 | 2025-09-10 |  750.00 | PAID   |
|        5 |           3 | 2025-09-20 | 2100.00 | PAID   |
|        6 |           4 | 2025-09-25 |  300.00 | PAID   |
|        8 |           2 | 2025-10-01 |  450.00 | PAID   |
+----------+-------------+------------+---------+--------+
```

```sql
SELECT * FROM orders WHERE status LIKE "%";
```
```
All 8 rows returned
```

```sql
SELECT * FROM orders WHERE status LIKE "_A%";
```
```
+----------+-------------+------------+---------+-----------+
| order_id | customer_id | order_date | amount  | status    |
+----------+-------------+------------+---------+-----------+
|        1 |           1 | 2025-09-01 | 1200.00 | PAID      |
|        2 |           1 | 2025-09-10 |  750.00 | PAID      |
|        4 |           3 | 2025-09-12 |  560.00 | CANCELLED |
|        5 |           3 | 2025-09-20 | 2100.00 | PAID      |
|        6 |           4 | 2025-09-25 |  300.00 | PAID      |
|        8 |           2 | 2025-10-01 |  450.00 | PAID      |
+----------+-------------+------------+---------+-----------+
```

```sql
SELECT * FROM orders WHERE status LIKE "p____%";
```
```
Empty set
```

## Joins

```sql
-- INNER JOIN
SELECT c.name, o.status 
FROM customers c 
INNER JOIN orders o ON c.customer_id = o.customer_id;
```
```
+-------------+-----------+
| name        | status    |
+-------------+-----------+
| Amit Sharma | PAID      |
| Amit Sharma | PAID      |
| Bhavna Iyer | NEW       |
| Bhavna Iyer | PAID      |
| Chetan Rao  | CANCELLED |
| Chetan Rao  | PAID      |
| Divya Menon | PAID      |
| Esha Khan   | NEW       |
+-------------+-----------+
```

```sql
SELECT c.name, o.status 
FROM customers c 
INNER JOIN orders o ON c.customer_id = o.customer_id 
WHERE o.status = "PAID";
```
```
+-------------+--------+
| name        | status |
+-------------+--------+
| Amit Sharma | PAID   |
| Amit Sharma | PAID   |
| Chetan Rao  | PAID   |
| Divya Menon | PAID   |
| Bhavna Iyer | PAID   |
+-------------+--------+
```

```sql
-- LEFT JOIN
SELECT c.name, o.status 
FROM orders o 
LEFT JOIN customers c ON c.customer_id = o.customer_id;
```
```
+-------------+-----------+
| name        | status    |
+-------------+-----------+
| Amit Sharma | PAID      |
| Amit Sharma | PAID      |
| Bhavna Iyer | NEW       |
| Chetan Rao  | CANCELLED |
| Chetan Rao  | PAID      |
| Divya Menon | PAID      |
| Esha Khan   | NEW       |
| Bhavna Iyer | PAID      |
+-------------+-----------+
```

```sql
-- Add customer with no orders
INSERT INTO customers VALUES (6,"Mohini","Mohini@gmail.com","nagpur");

-- RIGHT JOIN
SELECT c.name, o.status 
FROM orders o 
RIGHT JOIN customers c ON c.customer_id = o.customer_id;
```
```
+-------------+-----------+
| name        | status    |
+-------------+-----------+
| Amit Sharma | PAID      |
| Amit Sharma | PAID      |
| Bhavna Iyer | NEW       |
| Bhavna Iyer | PAID      |
| Chetan Rao  | CANCELLED |
| Chetan Rao  | PAID      |
| Divya Menon | PAID      |
| Esha Khan   | NEW       |
| Mohini      | NULL      |
+-------------+-----------+
```

## Aggregate Functions

```sql
SELECT MAX(order_date) FROM orders;
```
```
+-----------------+
| max(order_date) |
+-----------------+
| 2025-10-01      |
+-----------------+
```

```sql
SELECT c.name, MAX(o.order_date)
FROM customers c 
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.name;
```
```
+-------------+-------------------+
| name        | max(o.order_date) |
+-------------+-------------------+
| Amit Sharma | 2025-09-10        |
| Bhavna Iyer | 2025-10-01        |
| Chetan Rao  | 2025-09-20        |
| Divya Menon | 2025-09-25        |
| Esha Khan   | 2025-09-30        |
+-------------+-------------------+
```

```sql
SELECT c.customer_id, c.name, MAX(o.order_date)
FROM customers c 
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.name 
HAVING MONTH(MAX(o.order_date)) = 9;
```
```
+-------------+-------------+-------------------+
| customer_id | name        | max(o.order_date) |
+-------------+-------------+-------------------+
|           1 | Amit Sharma | 2025-09-10        |
|           3 | Chetan Rao  | 2025-09-20        |
|           4 | Divya Menon | 2025-09-25        |
|           5 | Esha Khan   | 2025-09-30        |
+-------------+-------------+-------------------+
```
