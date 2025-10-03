# Assignment: ER-Diagram Design

## Question 1: University Management System

### Problem Statement:

A university wants to maintain data about Students, Courses, Faculty, and Departments. The university management has provided the following requirements:

1. Each student has a unique roll number, name, email, phone number, and date of admission.
2. A student can enroll in many courses, and each course can have many students.
3. Each course has a course ID, course name, credits, and is offered by exactly one department.
4. Each department has a department ID, department name, and location.
5. Each faculty member has a faculty ID, name, designation, and email.
6. A faculty can teach many courses, but a course can be taught by only one faculty.
7. A student is assigned to exactly one faculty advisor.

---

### Tasks:

#### 1. Identify Entities & Attributes
List out all entities and their attributes (mention primary keys).

| **Entity**     | **Attributes**                                    | **Primary Key (PK)** |
|----------------|---------------------------------------------------|----------------------|
| **Student**    | RollNo, Name, Email, Phone, DateOfAdmission       | RollNo               |
| **Course**     | CourseID, CourseName, Credits, DepartmentID       | CourseID             |
| **Department** | DepartmentID, DepartmentName, Location            | DepartmentID         |
| **Faculty**    | FacultyID, Name, Designation, Email               | FacultyID            |

---

#### 2. Define Relationships
Define the relationships between entities along with cardinalities (1:1, 1:M, M:N).

| **Relationship** | **Entities Involved** | **Cardinality**    | **Relation**                                                                           |
|------------------|-----------------------|--------------------|----------------------------------------------------------------------------------------|
| **Enrolls**      | Student ↔ Course      | Many-to-Many (M:N) | A student can enroll in many courses; a course can have many students.                 |
| **OfferedBy**    | Course → Department   | Many-to-One (M:1)  | Each course is offered by exactly one department.                                      |
| **Teaches**      | Faculty → Course      | One-to-Many (1:M)  | Each faculty can teach many courses, but each course is taught by one faculty.         |
| **AdvisedBy**    | Student → Faculty     | Many-to-One (M:1)  | Each student has one faculty advisor.                                                  |

---

#### 3. Draw the ER Diagram
Use symbols for entities, attributes, relationships, and cardinalities.

![University Management System ER Diagram](221a1d02-bb9a-436b-841d-338959733fba.png)

---

#### 4. Convert to Tables (Optional Advanced Task)
Convert your ER diagram into relational schema (tables with PK and FK).

##### **Student Table**
```
Student
-------
RollNo (PK)
Name
Email
Phone
DateOfAdmission
FacultyID (FK) → References Faculty(FacultyID)
```

##### **Course Table**
```
Course
------
CourseID (PK)
CourseName
Credits
DepartmentID (FK) → References Department(DepartmentID)
FacultyID (FK) → References Faculty(FacultyID)
```

##### **Department Table**
```
Department
----------
DepartmentID (PK)
DepartmentName
Location
```

##### **Faculty Table**
```
Faculty
-------
FacultyID (PK)
Name
Designation
Email
```

##### **Enrollment Table** (Junction Table for M:N relationship)
```
Enrollment
----------
RollNo (FK) → References Student(RollNo)
CourseID (FK) → References Course(CourseID)
Primary Key: (RollNo, CourseID)
EnrollmentDate
```

## SQL Implementation Examples

#### Create Tables
```sql
-- Department Table
CREATE TABLE Department (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(100),
    Location VARCHAR(100)
);

-- Faculty Table
CREATE TABLE Faculty (
    FacultyID INT PRIMARY KEY,
    Name VARCHAR(100),
    Designation VARCHAR(50),
    Email VARCHAR(100)
);

-- Student Table
CREATE TABLE Student (
    RollNo INT PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    DateOfAdmission DATE,
    FacultyID INT,
    FOREIGN KEY (FacultyID) REFERENCES Faculty(FacultyID)
);

-- Course Table
CREATE TABLE Course (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(100),
    Credits INT,
    DepartmentID INT,
    FacultyID INT,
    FOREIGN KEY (DepartmentID) REFERENCES Department(DepartmentID),
    FOREIGN KEY (FacultyID) REFERENCES Faculty(FacultyID)
);

-- Enrollment Table (Junction Table)
CREATE TABLE Enrollment (
    RollNo INT,
    CourseID INT,
    EnrollmentDate DATE,
    PRIMARY KEY (RollNo, CourseID),
    FOREIGN KEY (RollNo) REFERENCES Student(RollNo),
    FOREIGN KEY (CourseID) REFERENCES Course(CourseID)
);
```

---

## Question 2: Online Shopping System

### Problem Statement:

An online shopping portal wants to maintain information about Customers, Orders, Products, Payments, and Delivery. The requirements are as follows:

1. Each customer has a customer ID, name, email, phone, and address.
2. A customer can place many orders, but each order belongs to only one customer.
3. An order has an order ID, order date, total amount, and status.
4. Each order can contain multiple products, and a product can appear in many orders (many-to-many relationship).
5. Each product has a product ID, name, description, price, and stock quantity.
6. Each order must have one payment, which includes payment ID, payment date, amount, and mode of payment (UPI, Credit Card, COD).
7. Each order is linked to a delivery, which includes delivery ID, delivery date, delivery status, and delivery address.

---

### Tasks:

#### 1. Identify Entities & Attributes
List entities and attributes (with primary keys).

| **Entity**   | **Attributes**                                                      | **Primary Key (PK)** |
|--------------|---------------------------------------------------------------------|----------------------|
| **Customer** | customerID, name, email, phone, address                             | customerID           |
| **Order**    | orderID, orderDate, totalAmount, status, customerID (FK)            | orderID              |
| **Product**  | productID, name, description, price, stockQuantity                  | productID            |
| **Payment**  | paymentID, paymentDate, amount, mode, orderID (FK)                  | paymentID            |
| **Delivery** | deliveryID, deliveryDate, deliveryStatus, deliveryAddress, orderID (FK) | deliveryID       |

---

#### 2. Define Relationships
Show all relationships with correct cardinalities.

| **Relationship**            | **Entities Involved** | **Cardinality**                |
|-----------------------------|-----------------------|--------------------------------|
| **Places**                  | Customer → Order      | 1 Customer → Many Orders       |
| **Contains**                | Order ↔ Product       | Many-to-Many                   |
| **Makes**                   | Order → Payment       | 1 Order → 1 Payment            |
| **Delivered By / Linked To** | Order → Delivery     | 1 Order → 1 Delivery           |

---

#### 3. Draw the ER Diagram
Use proper ER notation for entities, attributes, and relationships.


![Online Shopping System ER Diagram](26bc7e88-bc6f-42c9-8bc4-ca4b3a472899.png)

---

#### 4. Optional Task
Convert ER diagram into relational schema (tables with PK and FK).

##### **Customer Table**
```
Customer
--------
customerID (PK)
name
email
phone
address
```

##### **Order Table**
```
Order
-----
orderID (PK)
orderDate
totalAmount
status
customerID (FK) → References Customer(customerID)
```

##### **Product Table**
```
Product
-------
productID (PK)
name
description
price
stockQuantity
```

##### **Payment Table**
```
Payment
-------
paymentID (PK)
paymentDate
amount
mode
orderID (FK) → References Order(orderID)
```

##### **Delivery Table**
```
Delivery
--------
deliveryID (PK)
deliveryDate
deliveryStatus
deliveryAddress
orderID (FK) → References Order(orderID)
```

##### **OrderProduct Table** (Junction Table for M:N relationship)
```
OrderProduct
------------
orderID (FK) → References Order(orderID)
productID (FK) → References Product(productID)
Primary Key: (orderID, productID)
quantity
priceAtPurchase
```

---

## SQL Implementation Examples

#### Create Tables
```sql
-- Customer Table
CREATE TABLE Customer (
    customerID INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(15),
    address VARCHAR(255)
);

-- Order Table
CREATE TABLE `Order` (
    orderID INT PRIMARY KEY,
    orderDate DATE,
    totalAmount DECIMAL(10, 2),
    status VARCHAR(50),
    customerID INT,
    FOREIGN KEY (customerID) REFERENCES Customer(customerID)
);

-- Product Table
CREATE TABLE Product (
    productID INT PRIMARY KEY,
    name VARCHAR(100),
    description TEXT,
    price DECIMAL(10, 2),
    stockQuantity INT
);

-- Payment Table
CREATE TABLE Payment (
    paymentID INT PRIMARY KEY,
    paymentDate DATE,
    amount DECIMAL(10, 2),
    mode VARCHAR(50),
    orderID INT,
    FOREIGN KEY (orderID) REFERENCES `Order`(orderID)
);

-- Delivery Table
CREATE TABLE Delivery (
    deliveryID INT PRIMARY KEY,
    deliveryDate DATE,
    deliveryStatus VARCHAR(50),
    deliveryAddress VARCHAR(255),
    orderID INT,
    FOREIGN KEY (orderID) REFERENCES `Order`(orderID)
);

-- OrderProduct Table (Junction Table)
CREATE TABLE OrderProduct (
    orderID INT,
    productID INT,
    quantity INT,
    priceAtPurchase DECIMAL(10, 2),
    PRIMARY KEY (orderID, productID),
    FOREIGN KEY (orderID) REFERENCES `Order`(orderID),
    FOREIGN KEY (productID) REFERENCES Product(productID)
);
```
