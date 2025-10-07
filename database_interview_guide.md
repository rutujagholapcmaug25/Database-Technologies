# 📚 Complete Database Interview Guide

> **A comprehensive guide covering Database Fundamentals, RDBMS, SQL, ER Diagrams, and NoSQL for technical interviews**

---

## 📑 Table of Contents

1. [Database Fundamentals](#1-database-fundamentals)
2. [Database vs DBMS](#2-database-vs-dbms)
3. [Database Models](#3-database-models)
4. [RDBMS Deep Dive](#4-rdbms-deep-dive)
5. [Codd's 12 Rules](#5-codds-12-rules)
6. [Entity-Relationship Diagrams](#6-entity-relationship-diagrams)
7. [SQL Essentials](#7-sql-essentials)
8. [MongoDB (NoSQL)](#8-mongodb-nosql)

---

## 1. Database Fundamentals

### 🔹 What is Information and Data?

```
┌─────────────────────────────────────────────────────────────┐
│                    Knowledge Hierarchy                       │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│    📊 Data (Raw Facts)                                       │
│           ↓ Process & Organize                               │
│    📈 Information (Meaningful Data)                          │
│           ↓ Analyze & Understand                             │
│    🧠 Knowledge (Applied Information)                        │
│           ↓ Experience & Insight                             │
│    💡 Wisdom (Informed Decisions)                            │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

#### **Key Differences: Data vs Information**

| Aspect | Data | Information | Example |
|--------|------|-------------|---------|
| **Nature** | Raw, unprocessed facts; unstructured | Processed, organized, and meaningful | Data: `72°F, 68°F, 75°F` → Info: "Today's temperature is mild" |
| **Context** | Does not provide context | Provides context for understanding | Data: `85, 92, 78` → Info: "Class average is 85" |
| **Origin** | Collected from sensors, surveys, logs | Derived by processing/analyzing data | Data: Daily clicks → Info: "Top 5 visited pages" |
| **Purpose** | Independent; meaningless by itself | Purposeful; aids decision-making | Data: Sales per product → Info: "Focus on product X" |
| **Format** | Quantitative (numbers) or qualitative (text) | Combines both to generate insights | Data: Reviews → Info: "70% positive feedback" |
| **Storage** | Databases, spreadsheets, logs | Reports, dashboards, summaries | Data: Raw CSV → Info: BI dashboard |
| **Meaning** | Represents **"what happened"** | Represents **"what it means"** | Data: 500 visitors → Info: "Offer discount to convert 10%" |
| **Level** | Low-level knowledge (first stage) | Second-level knowledge (refined) | Data: `$50.25, $48.90` → Info: "Stock rose slightly" |

---

### 🔹 Structured vs Unstructured Data

```
┌──────────────────────────────────────────────────────────────────┐
│                     DATA CLASSIFICATION                           │
├──────────────────────────────────────────────────────────────────┤
│                                                                    │
│  STRUCTURED DATA              UNSTRUCTURED DATA                   │
│  ┌────────────────┐           ┌──────────────────┐               │
│  │  ID │ Name │Age│           │  📧 Emails       │               │
│  ├────┼──────┼───┤           │  📸 Images       │               │
│  │ 1  │ John │25 │           │  🎥 Videos       │               │
│  │ 2  │ Mary │30 │           │  📱 Social Posts │               │
│  └────────────────┘           │  📄 Documents    │               │
│                                └──────────────────┘               │
│  ✅ Easy to Query (SQL)        ⚙️ Needs AI/ML Tools              │
│  📊 Relational Databases       🗄️ NoSQL/Data Lakes               │
└──────────────────────────────────────────────────────────────────┘
```

| Aspect | Structured Data | Unstructured Data |
|--------|----------------|-------------------|
| **Definition** | Organized in predefined format (rows & columns) | Raw, unorganized, no predefined schema |
| **Storage** | RDBMS, Spreadsheets | Data lakes, NoSQL, file systems |
| **Schema** | Fixed schema required | No fixed schema |
| **Processing** | Easy - SQL queries | Complex - requires Big Data/AI/ML/NLP |
| **Querying** | Direct SQL queries | Cannot use SQL directly |
| **Examples** | Employee records, Bank transactions, Student database | Emails, Images, Videos, Social media posts |
| **Applications** | Banking, ERP, Sales tracking | Social media analysis, Image recognition, Sentiment analysis |

---

### 🔹 What is a Database?

**Definition:** A structured collection of related data stored electronically, managed by a DBMS, typically accessed using SQL.

```
┌─────────────────────────────────────────────────────────┐
│                  DATABASE ECOSYSTEM                      │
├─────────────────────────────────────────────────────────┤
│                                                           │
│  👤 Users ──→ [DBMS] ──→ 🗄️ Database (Tables)          │
│                  ↓                                        │
│            SQL Queries                                    │
│                  ↓                                        │
│         Data Operations (CRUD)                            │
│                                                           │
└─────────────────────────────────────────────────────────┘
```

#### **Advantages & Disadvantages**

| ✅ Advantages | ❌ Disadvantages |
|--------------|------------------|
| Organized storage in tables | Complex design & maintenance |
| Efficient data retrieval | Requires DBMS software |
| Reduces redundancy | Hardware resource intensive |
| Data integrity & accuracy | Needs skilled administrators |
| Security (authentication/authorization) | Higher initial cost |
| Multi-user concurrent access | |
| Scalability for large data | |

**💼 Real-World Applications:**
- Banking systems (transactions, accounts)
- Hospital management (patient records)
- E-commerce platforms (Amazon, Flipkart)
- Educational institutions (student records)

---

### 🔹 Database vs Spreadsheet

| Feature | Spreadsheet (Excel) | Database |
|---------|-------------------|----------|
| **Structure** | Simple rows & columns | Complex tables with relationships |
| **Manipulation** | Basic formulas, limited processing | Advanced SQL queries, joins, transactions |
| **Users** | Single user, limited collaboration | Multi-user with security controls |
| **Data Volume** | Small datasets | Large-scale data |
| **Security** | Minimal, manual checks | Strong constraints & integrity |
| **Use Case** | Personal budgets, simple lists | Enterprise apps, banking, analytics |

**Interview Tip:** *Spreadsheets are for individual, small-scale tasks; databases are for enterprise-level, multi-user, secure data management.*

---

### 🔹 Types of Databases

```
                    TYPES OF DATABASES
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
    RELATIONAL          NoSQL            SPECIALIZED
        │                  │                  │
   ┌────┴────┐        ┌────┴────┐       ┌────┴────┐
   │         │        │         │       │         │
 MySQL   PostgreSQL  MongoDB  Cassandra Cloud  Graph
 Oracle   MS SQL     Redis    Neo4j     DB     DB
```

| Database Type | Description | Examples | Use Case |
|--------------|-------------|----------|----------|
| **Relational (RDBMS)** | Data in tables with SQL | MySQL, PostgreSQL, Oracle | Banking, ERP systems |
| **NoSQL** | Unstructured/semi-structured data | MongoDB, Cassandra, Redis | Web apps, Big Data |
| **Cloud** | Hosted on cloud (DBaaS) | Amazon RDS, Azure SQL | Scalable remote access |
| **Graph** | Data as entities & relationships | Neo4j | Social networks, recommendations |
| **Data Warehouse** | Central repository for analytics | Snowflake, Redshift | Business intelligence |
| **OLTP** | Fast transactional processing | Oracle, SQL Server | Real-time transactions |
| **Hierarchical** | Tree-like structure | IBM IMS | File systems |
| **Document/JSON** | JSON format storage | MongoDB, CouchDB | Content management |

---

### 🔹 MySQL Database

**What is MySQL?**
- Open-source RDBMS based on SQL
- Optimized for web applications
- Cross-platform compatibility
- Handles millions of queries efficiently

**Key Features:**
```
┌────────────────────────────────────┐
│      MySQL Key Features            │
├────────────────────────────────────┤
│ ✓ Open-source & free              │
│ ✓ High performance & scalable     │
│ ✓ Secure data management          │
│ ✓ ACID compliant                  │
│ ✓ Supports replication            │
│ ✓ Cross-platform support          │
└────────────────────────────────────┘
```

**💼 Powers Major Platforms:**
Airbnb • Uber • LinkedIn • Facebook • Twitter • YouTube

---

## 2. Database vs DBMS

```
┌─────────────────────────────────────────────────────────┐
│                  DATABASE vs DBMS                        │
├─────────────────────────────────────────────────────────┤
│                                                           │
│   DATABASE                    DBMS                       │
│   ┌────────┐                ┌──────────┐                │
│   │ Data   │                │ Software │                │
│   │Storage │◄───manages─────│ Layer    │                │
│   │(Tables)│                │(MySQL)   │                │
│   └────────┘                └──────────┘                │
│                                                           │
└─────────────────────────────────────────────────────────┘
```

| Aspect | Database | DBMS |
|--------|----------|------|
| **Definition** | Collection of organized data | Software to manage databases |
| **Purpose** | Store data | Create, retrieve, update, delete (CRUD) |
| **Dependency** | Independent entity | Cannot exist without database |
| **Example** | Library catalog (data) | Library management software |
| **Nature** | Data container | Management tool |

**Interview Analogy:**
> **Database** = Library (books/data)  
> **DBMS** = Librarian (manages the library)

**✅ Advantages of DBMS:**
- Data security & access control
- Backup & recovery mechanisms
- Multi-user environment support
- Integrity constraint enforcement
- Reduced data redundancy

**💼 Real-World Examples:**
Railway reservation • Banking systems • Healthcare records • Inventory management

---

## 3. Database Models

### 📊 Model Comparison Overview

```
        DATABASE MODELS EVOLUTION
              Timeline
                │
    ┌───────────┼───────────┐
    │           │           │
 1960s       1970s       1980s-Present
    │           │           │
Hierarchical  Network   Relational
  Model       Model       Model
    │           │           │
  (IMS)     (CODASYL)    (MySQL)
                            │
                         2000s+
                            │
                         NoSQL
                        Models
```

---

### 1️⃣ Hierarchical Model

```
           Organization
                │
        ┌───────┴───────┐
     HR Dept        IT Dept
        │              │
    ┌───┴───┐      ┌───┴───┐
  Emp1   Emp2    Dev1   Dev2
```

| Feature | Details |
|---------|---------|
| **Structure** | Tree-like, parent-child relationship |
| **Advantage** | Fast access for hierarchy queries |
| **Disadvantage** | Difficult to restructure |
| **Example** | IBM IMS, XML data |
| **Application** | File systems, organizational charts |

---

### 2️⃣ Network Model

```
    Customer ────────> Account
        │                │
        └──> Loan <──────┘
```

| Feature | Details |
|---------|---------|
| **Structure** | Graph-like with nodes & edges |
| **Advantage** | Supports many-to-many relationships |
| **Disadvantage** | Complex navigation |
| **Example** | CODASYL databases |
| **Application** | Telecom, airline reservations |

---

### 3️⃣ Relational Model ⭐ (Most Important)

```
┌─────────────────┐         ┌──────────────────┐
│    STUDENTS     │         │     COURSES      │
├─────────────────┤         ├──────────────────┤
│ student_id (PK) │         │ course_id (PK)   │
│ name            │         │ title            │
│ email           │         │ credits          │
└─────────────────┘         └──────────────────┘
         │                           │
         └────── ENROLLMENTS ────────┘
                      │
         ┌────────────────────────┐
         │ student_id (FK)        │
         │ course_id (FK)         │
         │ semester               │
         │ grade                  │
         └────────────────────────┘
```

| Feature | Details |
|---------|---------|
| **Structure** | Data in tables (rows & columns) |
| **Advantage** | Easy to use, SQL support, ACID compliance |
| **Disadvantage** | Slower for very large unstructured data |
| **Example** | MySQL, PostgreSQL, Oracle |
| **Application** | Banking, ERP, education systems |

---

### 4️⃣ Object-Oriented Model

| Feature | Details |
|---------|---------|
| **Structure** | Data stored as objects (like OOP) |
| **Example** | db4o, ObjectDB |
| **Application** | CAD/CAM systems, multimedia databases |

---

### 5️⃣ NoSQL Models

```
┌──────────────────────────────────────────────────┐
│              NoSQL DATABASE TYPES                 │
├──────────────────────────────────────────────────┤
│                                                    │
│  Key-Value      Document      Column     Graph   │
│  ┌──────┐      ┌──────┐      ┌──────┐  ┌──────┐ │
│  │Redis │      │MongoDB│      │Cassan│  │Neo4j │ │
│  │      │      │CouchDB│      │-dra  │  │      │ │
│  └──────┘      └──────┘      └──────┘  └──────┘ │
│                                                    │
│  Fast cache    Flexible     Wide-column  Networks│
└──────────────────────────────────────────────────┘
```

| Type | Description | Example | Use Case |
|------|-------------|---------|----------|
| **Key-Value** | Simple key-value pairs | Redis, DynamoDB | Caching, session management |
| **Document** | JSON/BSON documents | MongoDB, CouchDB | Content management, catalogs |
| **Column** | Wide-column storage | Cassandra, HBase | Time-series data, analytics |
| **Graph** | Nodes & relationships | Neo4j, ArangoDB | Social networks, recommendations |

**💼 Applications:** Big Data, Real-time analytics, Social media platforms

---

## 4. RDBMS Deep Dive

### 🎯 What is RDBMS?

**Definition:** An advanced DBMS that stores data in tables (relations) with rows and columns, supporting relationships through keys.

```
┌────────────────────────────────────────────────────┐
│            RDBMS ARCHITECTURE                       │
├────────────────────────────────────────────────────┤
│                                                      │
│  Application Layer                                  │
│       ↓                                             │
│  SQL Interface                                      │
│       ↓                                             │
│  Query Processor                                    │
│       ↓                                             │
│  Storage Engine                                     │
│       ↓                                             │
│  Physical Database (Tables, Indexes)                │
│                                                      │
└────────────────────────────────────────────────────┘
```

---

### 🔑 Key Features of RDBMS

| Feature | Description | Benefit |
|---------|-------------|---------|
| **Tabular Storage** | Data in rows & columns | Easy to understand |
| **Relationships** | Primary & Foreign Keys | Data integrity |
| **SQL Support** | Standard query language | Universal access |
| **Normalization** | Reduces redundancy | Optimized storage |
| **ACID Properties** | Transaction reliability | Data consistency |
| **Constraints** | Data validation rules | Quality assurance |

---

### 📦 RDBMS Components

```
┌─────────────────────────────────────────────────────┐
│                 TABLE STRUCTURE                      │
├─────────────────────────────────────────────────────┤
│                                                       │
│  STUDENTS TABLE                                      │
│  ┌──────────┬─────────┬───────────────┬──────────┐ │
│  │student_id│  name   │    email      │   dob    │ │
│  │   (PK)   │         │   (UNIQUE)    │          │ │
│  ├──────────┼─────────┼───────────────┼──────────┤ │
│  │   101    │ John    │ john@edu.com  │1998-05-10│ │
│  │   102    │ Mary    │ mary@edu.com  │1999-03-15│ │
│  └──────────┴─────────┴───────────────┴──────────┘ │
│     ↑                                                │
│  Primary Key (Unique Identifier)                    │
└─────────────────────────────────────────────────────┘
```

| Component | Description |
|-----------|-------------|
| **Table** | Collection of rows and columns |
| **Row (Tuple)** | Single record in a table |
| **Column (Attribute)** | Property or field of data |
| **Primary Key** | Unique identifier for each row |
| **Foreign Key** | Reference to primary key in another table |
| **Schema** | Logical structure definition |

---

### ⚖️ DBMS vs RDBMS (Interview Critical)

| Aspect | DBMS | RDBMS | 🎯 Interview Insight |
|--------|------|-------|---------------------|
| **Full Form** | Database Management System | Relational Database Management System | RDBMS is advanced DBMS |
| **Data Storage** | Files/collections | Structured tables (rows & columns) | RDBMS provides structure |
| **Relationships** | Not supported | Supported via PK & FK | Enables logical data linking |
| **Normalization** | Not supported | Supported | Reduces redundancy |
| **Query Language** | May not support SQL | Fully supports SQL | SQL is standard |
| **Transactions** | Limited ACID | Fully ACID compliant | Ensures reliability |
| **Constraints** | Weak enforcement | Strong (PK, FK, UNIQUE, CHECK) | Guarantees data accuracy |
| **Redundancy** | High | Low (due to normalization) | Better memory optimization |
| **Security** | Basic | Advanced (roles, privileges) | Role-based access control |
| **Scalability** | Small applications | Large, complex, multi-user | Enterprise-ready |
| **Consistency** | May have issues | Strong consistency | Reliable across operations |
| **Backup** | Manual | Automated recovery | Crash recovery built-in |
| **Examples** | File systems, MS Access | MySQL, Oracle, PostgreSQL, SQL Server | RDBMS dominates industry |

---

### ✅ Advantages of RDBMS

```
┌─────────────────────────────────────┐
│     WHY CHOOSE RDBMS?               │
├─────────────────────────────────────┤
│ ✓ Intuitive table format            │
│ ✓ Strong data consistency           │
│ ✓ Complex SQL queries               │
│ ✓ Multi-user support                │
│ ✓ ACID transaction guarantee        │
│ ✓ Data security & access control    │
│ ✓ Scalable architecture             │
│ ✓ Industry standard                 │
└─────────────────────────────────────┘
```

---

### 🌍 Real-Life RDBMS Applications

| Domain | Use Case | Database |
|--------|----------|----------|
| **Banking** | Transaction management, accounts | Oracle, DB2 |
| **E-Commerce** | Products, orders, customers | MySQL, PostgreSQL |
| **Education** | Student records, courses, grades | SQL Server, MySQL |
| **Healthcare** | Patient records, billing, treatments | PostgreSQL, Oracle |
| **Telecom** | Customer data, billing, call logs | Oracle, SQL Server |

---

### 🔐 ACID Properties

```
┌──────────────────────────────────────────────────┐
│              ACID PROPERTIES                      │
├──────────────────────────────────────────────────┤
│                                                    │
│  A - Atomicity        All or nothing             │
│  C - Consistency      Valid state always         │
│  I - Isolation        Concurrent transactions    │
│  D - Durability       Permanent after commit     │
│                                                    │
└──────────────────────────────────────────────────┘
```

| Property | Description | Example |
|----------|-------------|---------|
| **Atomicity** | Transaction is all-or-nothing | Bank transfer: Both debit and credit must succeed |
| **Consistency** | Database moves from one valid state to another | Balance cannot be negative |
| **Isolation** | Concurrent transactions don't interfere | Two users withdrawing won't create race condition |
| **Durability** | Committed data persists even after system failure | Transaction logged and saved permanently |

---

### 📚 Popular RDBMS Examples

| RDBMS | Type | Key Features | Common Use |
|-------|------|--------------|------------|
| **MySQL** | Open Source | Fast, reliable, widely used | Web applications |
| **PostgreSQL** | Open Source | Advanced features, extensible | Complex queries, GIS |
| **Oracle** | Commercial | Enterprise-grade, robust | Large corporations |
| **SQL Server** | Commercial | Microsoft ecosystem | Windows-based apps |
| **MariaDB** | Open Source | MySQL fork, compatible | MySQL alternative |
| **SQLite** | Open Source | Embedded, serverless | Mobile apps, IoT |

---

## 5. Codd's 12 Rules

### 📜 Introduction

**Dr. E.F. Codd** proposed **13 rules (Rule 0 to Rule 12)** to define what makes a system truly relational.

```
┌──────────────────────────────────────────────────┐
│         CODD'S RULES HIERARCHY                    │
├──────────────────────────────────────────────────┤
│                                                    │
│  Rule 0: Foundation (Must use relational model)  │
│     ↓                                             │
│  Rules 1-4: Data Representation & Access         │
│     ↓                                             │
│  Rules 5-7: Data Manipulation                    │
│     ↓                                             │
│  Rules 8-11: Independence & Integrity            │
│     ↓                                             │
│  Rule 12: Security & Non-subversion              │
│                                                    │
└──────────────────────────────────────────────────┘
```

---

### 📋 Complete Table with Examples

| Rule No. | Rule Name | Explanation | Example |
|----------|-----------|-------------|---------|
| **Rule 0** | **Foundation Rule** | System must use relational features only (tables, keys, SQL) | MySQL/Oracle use tables and SQL, not direct file access |
| **Rule 1** | **Information Rule** | All data must be in tables (rows & columns) | `Student` table with columns: `RollNo`, `Name`, `Age` |
| **Rule 2** | **Guaranteed Access Rule** | Every data element accessible via: Table Name + Primary Key + Column | `SELECT Age FROM Student WHERE RollNo = 101;` |
| **Rule 3** | **Systematic Treatment of Null** | Must support NULL for missing/unknown data | Phone number not provided → store as `NULL`, not `0` |
| **Rule 4** | **Dynamic Online Catalog** | Metadata stored in tables, queryable via SQL | Query `INFORMATION_SCHEMA.TABLES` for table details |
| **Rule 5** | **Comprehensive Data Sub-language** | Must support complete language like SQL (DDL, DML, DCL) | `CREATE TABLE`, `INSERT`, `GRANT` commands |
| **Rule 6** | **View Updating Rule** | Updatable views must reflect in base tables | Update in `Student_View` reflects in `Student` table |
| **Rule 7** | **High-level Insert, Update, Delete** | Support set-level operations (not just one record) | `UPDATE Student SET Marks = Marks + 5 WHERE Dept = 'CSE';` |
| **Rule 8** | **Physical Data Independence** | Physical storage changes don't affect queries | Moving DB from HDD to SSD doesn't change SQL queries |
| **Rule 9** | **Logical Data Independence** | Schema changes don't break existing applications | Adding `Email` column doesn't affect old queries |
| **Rule 10** | **Integrity Independence** | Constraints stored in DB, not application code | `FOREIGN KEY (Dept_ID)` enforced automatically |
| **Rule 11** | **Distribution Independence** | Users unaware of data location (local/distributed) | `SELECT * FROM Student;` works same regardless of location |
| **Rule 12** | **Non-subversion Rule** | Low-level access must respect integrity rules | Direct access still enforces foreign key constraints |

---

### 🎯 Interview Quick Reference

```
┌────────────────────────────────────────────────┐
│       MOST FREQUENTLY ASKED RULES              │
├────────────────────────────────────────────────┤
│                                                 │
│  Rule 0: Foundation (Relational only)          │
│  Rule 2: Guaranteed Access (Table+PK+Column)   │
│  Rule 8: Physical Independence                 │
│  Rule 9: Logical Independence                  │
│  Rule 10: Integrity Independence               │
│                                                 │
└────────────────────────────────────────────────┘
```

**Interview Tip:** Focus on **Rule 0, 2, 8, 9, and 10** — these are most commonly asked!

---

## 6. Entity-Relationship Diagrams

### 🧩 What is an ER Diagram?

**Definition:** A visual representation of entities, their attributes, and relationships — used in database design before creating tables.

```
┌──────────────────────────────────────────────────┐
│            ER DIAGRAM WORKFLOW                    │
├──────────────────────────────────────────────────┤
│                                                    │
│  1. Identify Entities (Student, Course)           │
│            ↓                                       │
│  2. Define Attributes (name, email, credits)      │
│            ↓                                       │
│  3. Establish Relationships (ENROLLS_IN)          │
│            ↓                                       │
│  4. Draw ER Diagram                               │
│            ↓                                       │
│  5. Convert to Relational Schema (Tables)         │
│                                                    │
└──────────────────────────────────────────────────┘
```

---

### 🔑 Core Concepts

| Concept | Description | Example |
|---------|-------------|---------|
| **Entity** | Real-world object with data | `Student`, `Course`, `Department` |
| **Attribute** | Property of an entity | `Student.name`, `Course.credits` |
| **Relationship** | Association between entities | `Student ENROLLS_IN Course` |

---

### 📊 Attribute Types

```
              ATTRIBUTE TYPES
                    │
        ┌───────────┼───────────┐
        │           │           │
     Simple     Composite   Multi-Valued
        │           │           │
     (Name)  (Full_Name)   (Phone)
              ↓
        {First, Last}
```

| Type | Description | Example |
|------|-------------|---------|
| **Simple** | Cannot be divided | `Name`, `RollNo` |
| **Composite** | Can be subdivided | `Name → {First_Name, Last_Name}` |
| **Single-Valued** | One value only | `Email` |
| **Multi-Valued** | Multiple values | `Phone {+91..., +44...}` |
| **Derived** | Calculated from another | `Age` from `Date_of_Birth` |
| **Key Attribute** | Unique identifier | `student_id` (underlined) |

---

### 🔗 Relationship Types

```
┌────────────────────────────────────────────────┐
│         RELATIONSHIP CARDINALITY                │
├────────────────────────────────────────────────┤
│                                                 │
│  1:1 (One-to-One)                              │
│  Person ──────── Passport                      │
│                                                 │
│  1:N (One-to-Many)                             │
│  Teacher ──────< Students                      │
│                                                 │
│  M:N (Many-to-Many)                            │
│  Students >────< Courses                       │
│                                                 │
└────────────────────────────────────────────────┘
```

| Type | Description | Example |
|------|-------------|---------|
| **1:1** | One entity relates to exactly one other | Person has one Passport |
| **1:N** | One entity relates to many | Teacher teaches many Students |
| **M:N** | Many entities relate to many | Students enroll in many Courses |
| **Total Participation** | Every entity must participate | Every Student must enroll in Course |
| **Partial Participation** | Some may not participate | Teacher may not teach any Course |

---

### 🛠️ Advanced ER Constructs

| Construct | Description | Example |
|-----------|-------------|---------|
| **Weak Entity** | Cannot exist independently; depends on strong entity | `Dependent` depends on `Employee` |
| **Identifying Relationship** | Links weak entity to strong entity | `Dependent OF Employee` |
| **ISA (Generalization)** | Supertype-subtype relationship | `Person ISA {Student, Teacher}` |
| **Aggregation** | Relationship treated as entity | `Project-Employee` relationship |

---

### 🎨 ER Diagram Symbols

```
┌─────────────────────────────────────────────┐
│          ER DIAGRAM NOTATION                 │
├─────────────────────────────────────────────┤
│                                              │
│  ┌─────────┐        Entity                  │
│  │ Student │                                │
│