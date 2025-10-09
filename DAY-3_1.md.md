# MySQL Constraint Errors — Complete Guide with Solutions

---

## Database Setup

```sql
DROP DATABASE IF EXISTS shop_practice;
CREATE DATABASE shop_practice;
USE shop_practice;

-- Customers table with PRIMARY KEY, NOT NULL, and UNIQUE constraints
CREATE TABLE customers (
  customer_id INT AUTO_INCREMENT PRIMARY KEY,
  name        VARCHAR(50) NOT NULL,
  email       VARCHAR(120) UNIQUE
);

-- Products table with PRIMARY KEY, NOT NULL, and UNIQUE constraints
CREATE TABLE products (
  product_id INT AUTO_INCREMENT PRIMARY KEY,
  name       VARCHAR(80) NOT NULL,
  sku        VARCHAR(30) UNIQUE
);

-- Orders table with FOREIGN KEY constraints
CREATE TABLE orders (
  order_id    INT AUTO_INCREMENT PRIMARY KEY,
  customer_id INT,
  product_id  INT,
  quantity    INT DEFAULT 1,
  CONSTRAINT fk_orders_customer
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
    ON DELETE RESTRICT ON UPDATE CASCADE,
  CONSTRAINT fk_orders_product
    FOREIGN KEY (product_id) REFERENCES products(product_id)
    ON DELETE RESTRICT ON UPDATE CASCADE
);
```

---

## Sample Data Insertion

```sql
-- Successfully inserted customers
INSERT INTO customers (name, email) VALUES
('Amit', 'amit@shop.com'),
('Bhavna', 'bhavna@shop.com');

-- Successfully inserted products
INSERT INTO products (name, sku) VALUES
('Notebook A5', 'SKU-1001'),
('Blue Pen', 'SKU-2001');

-- Successfully inserted orders
INSERT INTO orders (customer_id, product_id, quantity) VALUES
(1, 1, 2),
(2, 2, 5);
```

---

## Common Constraint Errors & Solutions

### 1. PRIMARY KEY Violation (Error 1062)

**Error Query:**
```sql
INSERT INTO customers (customer_id, name, email) 
VALUES (1, 'DuplicatePK', 'dup1@shop.com');
```

**Error Message:**
```
ERROR 1062 (23000): Duplicate entry '1' for key 'customers.PRIMARY'
```

**Reason:**
- PRIMARY KEY must be unique across all rows
- customer_id = 1 already exists in the table
- Attempting to insert a duplicate PRIMARY KEY value

**Concept:**
- PRIMARY KEY constraint ensures each row has a unique identifier
- Cannot have two rows with the same PRIMARY KEY value
- PRIMARY KEY = UNIQUE + NOT NULL

**Solution:**
```sql
-- Let AUTO_INCREMENT handle the ID automatically
INSERT INTO customers (name, email) 
VALUES ('DuplicatePK', 'dup1@shop.com');

-- OR use a unique ID that doesn't exist
INSERT INTO customers (customer_id, name, email) 
VALUES (3, 'DuplicatePK', 'dup1@shop.com');
```

---

### 2. UNIQUE Constraint Violation (Error 1062)

**Error Query:**
```sql
INSERT INTO customers (name, email) 
VALUES ('Another Amit', 'amit@shop.com');
```

**Error Message:**
```
ERROR 1062 (23000): Duplicate entry 'amit@shop.com' for key 'customers.email'
```

**Reason:**
- UNIQUE constraint prevents duplicate values in the email column
- Email 'amit@shop.com' already exists for another customer
- Each email must be unique across all customers

**Concept:**
- UNIQUE constraint ensures no duplicate values in a column
- Commonly used for emails, usernames, phone numbers, SKUs
- Allows NULL values (one NULL is permitted)

**Solution:**
```sql
-- Use a different email address
INSERT INTO customers (name, email) 
VALUES ('Another Amit', 'another.amit@shop.com');

-- OR update the existing record instead
UPDATE customers 
SET name = 'Another Amit' 
WHERE email = 'amit@shop.com';
```

---

**Another UNIQUE Violation Example:**

**Error Query:**
```sql
INSERT INTO products (name, sku) 
VALUES ('Black Pen', 'SKU-2001');
```

**Error Message:**
```
ERROR 1062 (23000): Duplicate entry 'SKU-2001' for key 'products.sku'
```

**Solution:**
```sql
-- Use a unique SKU
INSERT INTO products (name, sku) 
VALUES ('Black Pen', 'SKU-2002');
```

---

### 3. NOT NULL Constraint Violation (Error 1048)

**Error Query 1:**
```sql
INSERT INTO customers (name, email) 
VALUES (NULL, 'nullname@shop.com');
```

**Error Message:**
```
ERROR 1048 (23000): Column 'name' cannot be null
```

**Error Query 2:**
```sql
INSERT INTO products (name, sku) 
VALUES (NULL, 'SKU-3001');
```

**Error Message:**
```
ERROR 1048 (23000): Column 'name' cannot be null
```

**Reason:**
- NOT NULL constraint requires a value in the column
- Cannot insert NULL into columns marked as NOT NULL
- Name column is mandatory in both tables

**Concept:**
- NOT NULL ensures mandatory fields always have data
- Prevents incomplete or invalid records
- Used for critical business data (names, IDs, dates)

**Solution:**
```sql
-- Provide a valid non-NULL value
INSERT INTO customers (name, email) 
VALUES ('Valid Name', 'nullname@shop.com');

INSERT INTO products (name, sku) 
VALUES ('Black Pen', 'SKU-3001');

-- OR use a default value if appropriate
INSERT INTO customers (name, email) 
VALUES (COALESCE(NULL, 'Unknown'), 'nullname@shop.com');
```

---

### 4. FOREIGN KEY Constraint Violation - INSERT (Error 1452)

**Error Query 1:**
```sql
INSERT INTO orders (customer_id, product_id, quantity) 
VALUES (99, 1, 1);
```

**Error Message:**
```
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails 
(`shop_practice`.`orders`, CONSTRAINT `fk_orders_customer` 
FOREIGN KEY (`customer_id`) REFERENCES `customers` (`customer_id`) 
ON DELETE RESTRICT ON UPDATE CASCADE)
```

**Error Query 2:**
```sql
INSERT INTO orders (customer_id, product_id, quantity) 
VALUES (1, 99, 1);
```

**Error Message:**
```
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails 
(`shop_practice`.`orders`, CONSTRAINT `fk_orders_product` 
FOREIGN KEY (`product_id`) REFERENCES `products` (`product_id`) 
ON DELETE RESTRICT ON UPDATE CASCADE)
```

**Reason:**
- FOREIGN KEY enforces referential integrity between tables
- customer_id = 99 does not exist in customers table
- product_id = 99 does not exist in products table
- Cannot reference non-existent parent records

**Concept:**
- FOREIGN KEY links child table to parent table
- Child table (orders) references parent tables (customers, products)
- Ensures data consistency across related tables
- Maintains referential integrity

**Solution:**
```sql
-- First, check which customers exist
SELECT customer_id FROM customers;

-- Use existing customer_id and product_id
INSERT INTO orders (customer_id, product_id, quantity) 
VALUES (1, 1, 1);

-- OR create the customer/product first, then create the order
INSERT INTO customers (name, email) 
VALUES ('New Customer', 'newcust@shop.com');

INSERT INTO orders (customer_id, product_id, quantity) 
VALUES (LAST_INSERT_ID(), 1, 1);
```

---

### 5. FOREIGN KEY Constraint Violation - UPDATE (Error 1452)

**Error Query:**
```sql
UPDATE orders 
SET customer_id = 77 
WHERE order_id = 1;
```

**Error Message:**
```
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails 
(`shop_practice`.`orders`, CONSTRAINT `fk_orders_customer` 
FOREIGN KEY (`customer_id`) REFERENCES `customers` (`customer_id`) 
ON DELETE RESTRICT ON UPDATE CASCADE)
```

**Reason:**
- Attempting to update customer_id to 77
- customer_id = 77 does not exist in customers table
- FOREIGN KEY prevents orphaned records

**Concept:**
- UPDATE operations must maintain referential integrity
- Cannot change FK to reference non-existent parent
- Protects data consistency during modifications

**Solution:**
```sql
-- Update to an existing customer_id
UPDATE orders 
SET customer_id = 2 
WHERE order_id = 1;

-- OR create the customer first
INSERT INTO customers (customer_id, name, email) 
VALUES (77, 'Customer 77', 'cust77@shop.com');

-- Then update the order
UPDATE orders 
SET customer_id = 77 
WHERE order_id = 1;
```

---

### 6. FOREIGN KEY Constraint Violation - DELETE (Error 1451)

**Error Query 1:**
```sql
DELETE FROM customers 
WHERE customer_id = 1;
```

**Error Message:**
```
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails 
(`shop_practice`.`orders`, CONSTRAINT `fk_orders_customer` 
FOREIGN KEY (`customer_id`) REFERENCES `customers` (`customer_id`) 
ON DELETE RESTRICT ON UPDATE CASCADE)
```

**Error Query 2:**
```sql
DELETE FROM products 
WHERE product_id = 2;
```

**Error Message:**
```
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails 
(`shop_practice`.`orders`, CONSTRAINT `fk_orders_product` 
FOREIGN KEY (`product_id`) REFERENCES `products` (`product_id`) 
ON DELETE RESTRICT ON UPDATE CASCADE)
```

**Reason:**
- ON DELETE RESTRICT prevents deletion of parent records with child references
- customer_id = 1 is referenced by orders (order_id = 1)
- product_id = 2 is referenced by orders (order_id = 2)
- Cannot delete parent records that have dependent child records

**Concept:**
- ON DELETE RESTRICT protects against accidental data loss
- Prevents orphaned records in child tables
- Forces proper cleanup of related data

**Solution:**

**Option 1: Delete child records first**
```sql
-- Delete the orders first
DELETE FROM orders WHERE customer_id = 1;

-- Then delete the customer
DELETE FROM customers WHERE customer_id = 1;
```

**Option 2: Change FK constraint to CASCADE**
```sql
-- Drop existing constraint
ALTER TABLE orders 
DROP FOREIGN KEY fk_orders_customer;

-- Recreate with ON DELETE CASCADE
ALTER TABLE orders 
ADD CONSTRAINT fk_orders_customer
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
  ON DELETE CASCADE ON UPDATE CASCADE;

-- Now deleting customer will automatically delete their orders
DELETE FROM customers WHERE customer_id = 1;
```

**Option 3: Set FK to NULL**
```sql
-- Modify orders table to allow NULL
ALTER TABLE orders 
MODIFY customer_id INT NULL;

-- Change constraint to SET NULL
ALTER TABLE orders 
DROP FOREIGN KEY fk_orders_customer;

ALTER TABLE orders 
ADD CONSTRAINT fk_orders_customer
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
  ON DELETE SET NULL ON UPDATE CASCADE;

-- Now deleting customer will set orders.customer_id to NULL
DELETE FROM customers WHERE customer_id = 1;
```

---

## Foreign Key Actions Summary

| Action | Description | Use Case |
|--------|-------------|----------|
| **RESTRICT** | Prevents deletion/update of parent if child exists | Protect critical data |
| **CASCADE** | Automatically deletes/updates child records | Master-detail relationships |
| **SET NULL** | Sets FK to NULL when parent is deleted/updated | Optional relationships |
| **NO ACTION** | Similar to RESTRICT (checked at end of statement) | Default behavior |
| **SET DEFAULT** | Sets FK to default value (not supported in InnoDB) | Rare use cases |

---

## Error Code Quick Reference

| Error Code | Error Type | Common Causes |
|------------|------------|---------------|
| **1062** | Duplicate entry | PRIMARY KEY or UNIQUE violation |
| **1048** | Column cannot be null | NOT NULL constraint violation |
| **1452** | Cannot add/update child row | FOREIGN KEY reference doesn't exist |
| **1451** | Cannot delete/update parent row | Child records exist with RESTRICT/NO ACTION |

---

## Best Practices

### 1. Designing Constraints
- Use PRIMARY KEY for unique identifiers
- Apply UNIQUE for naturally unique fields (email, username, SKU)
- Use NOT NULL for mandatory business data
- Define FOREIGN KEYs for relationships

### 2. Handling Deletions
- Choose appropriate ON DELETE action based on business logic
- Use RESTRICT for critical parent records
- Use CASCADE for dependent child records
- Consider SET NULL for optional relationships

### 3. Error Prevention
- Check if parent records exist before inserting child records
- Validate unique values before insertion
- Ensure required fields have values
- Delete child records before parent records (with RESTRICT)

### 4. Application Development
- Handle constraint errors gracefully in application code
- Provide meaningful error messages to users
- Use transactions for multi-table operations
- Implement proper validation before database operations

---

## Testing Constraints

```sql
-- Test PRIMARY KEY
SELECT * FROM customers WHERE customer_id = 1;

-- Test UNIQUE
SELECT * FROM customers WHERE email = 'amit@shop.com';

-- Test FOREIGN KEY relationships
SELECT c.name, o.order_id, p.name AS product_name
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN products p ON o.product_id = p.product_id;

-- Check constraint definitions
SHOW CREATE TABLE orders;

-- View foreign key information
SELECT 
  CONSTRAINT_NAME,
  TABLE_NAME,
  COLUMN_NAME,
  REFERENCED_TABLE_NAME,
  REFERENCED_COLUMN_NAME
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE TABLE_SCHEMA = 'shop_practice'
  AND REFERENCED_TABLE_NAME IS NOT NULL;
```

---

## Key Takeaways

1. **Constraints maintain data integrity** - They prevent invalid data from entering the database
2. **Error codes indicate specific violations** - Understanding error codes helps debug issues quickly
3. **Foreign keys enforce relationships** - They ensure referential integrity between tables
4. **Plan deletion strategies** - Choose appropriate ON DELETE actions during schema design
5. **Test thoroughly** - Verify constraint behavior before deploying to production