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
│  └─────────┘                                │
│                                              │
│  ╔═════════╗        Weak Entity             │
│  ║Dependent║                                │
│  ╚═════════╝                                │
│                                              │
│  ◇  Relationship                            │
│  ◇  ENROLLS_IN                              │
│                                              │
│  ◈  Identifying Relationship                │
│                                              │
│  ⃝  Attribute                                │
│  ⃝  name                                     │
│                                              │
│  ◎  Multi-valued Attribute                  │
│  ◎  {phone}                                 │
│                                              │
│  ⃝  Derived Attribute (dashed)              │
│  ---  age                                   │
│                                              │
│  ⃝  Key Attribute (underlined)              │
│  _student_id_                               │
│                                              │
└─────────────────────────────────────────────┘
```

| Symbol | Representation |
|--------|----------------|
| Rectangle | Entity |
| Double Rectangle | Weak Entity |
| Diamond | Relationship |
| Double Diamond | Identifying Relationship |
| Oval | Attribute |
| Double Oval | Multi-valued Attribute |
| Dashed Oval | Derived Attribute |
| Underlined Text | Key Attribute |
| Lines | Connections |

---

### 📊 Complete ER Diagram Example

**Scenario:** University Database with Students, Courses, and Faculty

```
          ┌─────────────┐
          │  STUDENT    │
          ├─────────────┤
          │ student_id  │───── (PK, underlined)
          │ name        │
          │ email       │───── (UNIQUE)
          │ {phone}     │───── (Multi-valued)
          │ age         │───── (Derived)
          └──────┬──────┘
                 │
                 │ M
                 │
         ┌───────▼────────┐
         │   ENROLLS_IN   │
         │                │
         │ semester       │───── (Relationship Attribute)
         │ grade          │
         └───────┬────────┘
                 │ N
                 │
          ┌──────▼──────┐
          │   COURSE    │
          ├─────────────┤
          │ course_id   │───── (PK)
          │ title       │
          │ credits     │
          └──────┬──────┘
                 │ N
                 │
         ┌───────▼────────┐
         │   TAUGHT_BY    │
         └───────┬────────┘
                 │ 1
                 │
          ┌──────▼──────┐
          │   FACULTY   │
          ├─────────────┤
          │ faculty_id  │───── (PK)
          │ name        │
          │ department  │
          └─────────────┘
```

---

### 🔄 Relational Mapping (ER to Tables)

**Converting ER Diagram to Relational Schema:**

```sql
-- Entity: Student
┌────────────────────────────────────────────────┐
│ TABLE: Student                                 │
├────────────────────────────────────────────────┤
│ student_id (PK)  │ name    │ email   │ dob    │
├──────────────────┼─────────┼─────────┼────────┤
│ 101              │ John    │ j@...   │ 1998.. │
│ 102              │ Mary    │ m@...   │ 1999.. │
└────────────────────────────────────────────────┘

-- Entity: Course
┌────────────────────────────────────────────────┐
│ TABLE: Course                                  │
├────────────────────────────────────────────────┤
│ course_id (PK)   │ title          │ credits   │
├──────────────────┼────────────────┼───────────┤
│ CSE101           │ Databases      │ 4         │
│ CSE102           │ Networks       │ 3         │
└────────────────────────────────────────────────┘

-- M:N Relationship: ENROLLS_IN
┌────────────────────────────────────────────────┐
│ TABLE: Enrollment (Bridge Table)              │
├────────────────────────────────────────────────┤
│ student_id │ course_id │ semester │ grade     │
│    (FK)    │   (FK)    │          │           │
├────────────┼───────────┼──────────┼───────────┤
│ 101        │ CSE101    │ Fall2024 │ A         │
│ 101        │ CSE102    │ Fall2024 │ B+        │
│ 102        │ CSE101    │ Fall2024 │ A-        │
└────────────────────────────────────────────────┘
-- Composite PK: (student_id, course_id, semester)
```

**Mapping Rules:**

| ER Concept | Relational Mapping |
|------------|-------------------|
| **Entity** | Table |
| **Attribute** | Column |
| **Primary Key** | Primary Key constraint |
| **1:1 Relationship** | Foreign Key in either table |
| **1:N Relationship** | Foreign Key on "many" side |
| **M:N Relationship** | New bridge/junction table |
| **Multi-valued Attribute** | Separate table |
| **Composite Attribute** | Multiple columns or separate table |
| **Weak Entity** | Table with FK to strong entity |

---

### 💼 Real-Life ER Applications

| Domain | Entities | Relationships | Use Case |
|--------|----------|---------------|----------|
| **Banking** | Customer, Account, Transaction | Customer HAS Account, Account HAS Transactions | Account management |
| **E-Commerce** | Customer, Order, Product | Customer PLACES Order, Order CONTAINS Products | Online shopping |
| **University** | Student, Course, Faculty | Student ENROLLS Course, Faculty TEACHES Course | Academic management |
| **Hospital** | Patient, Doctor, Appointment | Patient BOOKS Appointment WITH Doctor | Healthcare system |
| **Library** | Member, Book, Loan | Member BORROWS Book | Book tracking |

---

### 🎯 Interview Tips for ER Diagrams

```
┌──────────────────────────────────────────────┐
│     COMMON ER DIAGRAM INTERVIEW QUESTIONS    │
├──────────────────────────────────────────────┤
│                                               │
│ 1. Draw ER for Library Management System     │
│ 2. Explain weak entity with example          │
│ 3. Convert given ER to relational schema     │
│ 4. Difference between 1:N and M:N            │
│ 5. What is total vs partial participation?   │
│ 6. How to handle multi-valued attributes?    │
│ 7. Design ER for e-commerce system           │
│                                               │
└──────────────────────────────────────────────┘
```

**Pro Tips:**
- Always identify entities first, then relationships
- Use meaningful names for entities and relationships
- Mark key attributes clearly
- Show cardinality ratios explicitly
- Practice converting ER to SQL tables

---

## 7. SQL Essentials

### 🗃️ What is SQL?

**SQL (Structured Query Language)** is the standard language for managing relational databases, enabling CRUD operations (Create, Read, Update, Delete).

```
┌──────────────────────────────────────────────┐
│              SQL ECOSYSTEM                    │
├──────────────────────────────────────────────┤
│                                               │
│  User ──→ SQL Query ──→ DBMS ──→ Database   │
│                ↓                              │
│           Results Returned                    │
│                                               │
└──────────────────────────────────────────────┘
```

---

### 📚 SQL Command Categories

```
                    SQL COMMANDS
                         │
        ┌────────────────┼────────────────┐
        │                │                │
       DDL             DML              DCL
        │                │                │
    (Structure)      (Data)          (Access)
        │                │                │
    CREATE           SELECT            GRANT
    ALTER            INSERT           REVOKE
    DROP             UPDATE
    TRUNCATE         DELETE
                         │
                        TCL
                         │
                   (Transaction)
                         │
                      COMMIT
                     ROLLBACK
                     SAVEPOINT
```

| Category | Full Form | Commands | Purpose | Example |
|----------|-----------|----------|---------|---------|
| **DDL** | Data Definition Language | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` | Define/modify structure | `CREATE TABLE students` |
| **DML** | Data Manipulation Language | `SELECT`, `INSERT`, `UPDATE`, `DELETE` | Manipulate data | `INSERT INTO students` |
| **DCL** | Data Control Language | `GRANT`, `REVOKE` | Control access | `GRANT SELECT ON students` |
| **TCL** | Transaction Control Language | `COMMIT`, `ROLLBACK`, `SAVEPOINT` | Manage transactions | `COMMIT;` |

---

### 🔑 Key SQL Concepts

```
┌──────────────────────────────────────────────┐
│         SQL FUNDAMENTAL CONCEPTS              │
├──────────────────────────────────────────────┤
│                                               │
│  PRIMARY KEY ──→ Unique Identifier           │
│  FOREIGN KEY ──→ References another table    │
│  CONSTRAINTS ──→ Data integrity rules        │
│  JOINS       ──→ Combine multiple tables     │
│  INDEXES     ──→ Fast data retrieval         │
│  VIEWS       ──→ Virtual tables              │
│                                               │
└──────────────────────────────────────────────┘
```

| Concept | Description | Example |
|---------|-------------|---------|
| **Primary Key (PK)** | Uniquely identifies each record | `student_id` |
| **Foreign Key (FK)** | Links to PK in another table | `course_id` references `Course(course_id)` |
| **Constraints** | Rules to maintain data integrity | `NOT NULL`, `UNIQUE`, `CHECK`, `DEFAULT` |
| **Joins** | Combine rows from multiple tables | `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN` |
| **Index** | Speeds up data retrieval | `CREATE INDEX idx_name ON students(name)` |
| **View** | Virtual table based on query | `CREATE VIEW active_students AS...` |

---

### 📊 SQL Data Types

| Category | Data Types | Description | Example |
|----------|-----------|-------------|---------|
| **Numeric** | `INT`, `BIGINT`, `DECIMAL(p,s)`, `FLOAT`, `DOUBLE` | Numbers | `age INT`, `salary DECIMAL(10,2)` |
| **String** | `CHAR(n)`, `VARCHAR(n)`, `TEXT` | Text data | `name VARCHAR(100)` |
| **Date/Time** | `DATE`, `TIME`, `DATETIME`, `TIMESTAMP` | Temporal data | `dob DATE`, `created_at TIMESTAMP` |
| **Boolean** | `BOOLEAN` | True/False | `is_active BOOLEAN` |
| **Binary** | `BLOB`, `BINARY` | Binary data | `profile_pic BLOB` |

---

### 🛠️ SQL Constraints

```
┌──────────────────────────────────────────────┐
│            DATA INTEGRITY CONSTRAINTS         │
├──────────────────────────────────────────────┤
│                                               │
│  NOT NULL     ──→ Cannot be empty            │
│  UNIQUE       ──→ All values different       │
│  PRIMARY KEY  ──→ NOT NULL + UNIQUE          │
│  FOREIGN KEY  ──→ References another table   │
│  CHECK        ──→ Custom condition           │
│  DEFAULT      ──→ Default value              │
│                                               │
└──────────────────────────────────────────────┘
```

| Constraint | Purpose | Example |
|------------|---------|---------|
| `NOT NULL` | Column cannot have NULL | `name VARCHAR(100) NOT NULL` |
| `UNIQUE` | All values must be unique | `email VARCHAR(150) UNIQUE` |
| `PRIMARY KEY` | Unique identifier | `student_id INT PRIMARY KEY` |
| `FOREIGN KEY` | Enforces referential integrity | `FOREIGN KEY (dept_id) REFERENCES departments(id)` |
| `CHECK` | Custom validation | `age INT CHECK (age >= 18)` |
| `DEFAULT` | Default value if not provided | `status VARCHAR(20) DEFAULT 'active'` |

---

### 📝 SQL DDL Examples

#### **CREATE TABLE**

```sql
CREATE TABLE students (
    student_id    INT PRIMARY KEY AUTO_INCREMENT,
    name          VARCHAR(100) NOT NULL,
    email         VARCHAR(150) UNIQUE,
    dob           DATE,
    age           INT CHECK (age >= 18),
    status        VARCHAR(20) DEFAULT 'active',
    created_at    TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE courses (
    course_id     INT PRIMARY KEY,
    title         VARCHAR(120) NOT NULL,
    credits       INT CHECK (credits BETWEEN 1 AND 6),
    description   TEXT
);

CREATE TABLE enrollments (
    enrollment_id INT PRIMARY KEY AUTO_INCREMENT,
    student_id    INT NOT NULL,
    course_id     INT NOT NULL,
    semester      VARCHAR(10) NOT NULL,
    grade         VARCHAR(2),
    enrolled_date DATE DEFAULT CURRENT_DATE,
    
    FOREIGN KEY (student_id) REFERENCES students(student_id) 
        ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
        ON DELETE RESTRICT ON UPDATE CASCADE,
        
    UNIQUE (student_id, course_id, semester)
);
```

#### **ALTER TABLE**

```sql
-- Add new column
ALTER TABLE students ADD COLUMN phone VARCHAR(15);

-- Modify column
ALTER TABLE students MODIFY COLUMN name VARCHAR(150);

-- Drop column
ALTER TABLE students DROP COLUMN status;

-- Add constraint
ALTER TABLE students ADD CONSTRAINT chk_age CHECK (age >= 18);

-- Add foreign key
ALTER TABLE enrollments 
ADD FOREIGN KEY (student_id) REFERENCES students(student_id);
```

#### **DROP & TRUNCATE**

```sql
-- Delete entire table structure
DROP TABLE enrollments;

-- Delete all records, keep structure
TRUNCATE TABLE enrollments;
```

---

### 📥 SQL DML Examples

#### **INSERT**

```sql
-- Single record
INSERT INTO students (name, email, dob, age)
VALUES ('John Doe', 'john@edu.com', '2000-05-15', 24);

-- Multiple records
INSERT INTO students (name, email, dob, age) VALUES
    ('Mary Smith', 'mary@edu.com', '1999-08-20', 25),
    ('Robert Johnson', 'robert@edu.com', '2001-03-10', 23),
    ('Sarah Williams', 'sarah@edu.com', '2000-11-25', 24);

-- Insert from another table
INSERT INTO archived_students
SELECT * FROM students WHERE status = 'graduated';
```

#### **SELECT**

```sql
-- Basic select
SELECT * FROM students;

-- Select specific columns
SELECT name, email FROM students;

-- With WHERE clause
SELECT name, age FROM students WHERE age > 22;

-- With multiple conditions
SELECT * FROM students 
WHERE age > 20 AND status = 'active';

-- Pattern matching
SELECT * FROM students WHERE name LIKE 'J%';

-- Sorting
SELECT * FROM students ORDER BY age DESC;

-- Limiting results
SELECT * FROM students LIMIT 10;

-- Distinct values
SELECT DISTINCT status FROM students;

-- Aggregate functions
SELECT COUNT(*) as total_students FROM students;
SELECT AVG(age) as avg_age FROM students;
SELECT MAX(age) as oldest, MIN(age) as youngest FROM students;

-- GROUP BY
SELECT status, COUNT(*) as count 
FROM students 
GROUP BY status;

-- HAVING clause
SELECT status, COUNT(*) as count 
FROM students 
GROUP BY status 
HAVING count > 5;
```

#### **UPDATE**

```sql
-- Update single record
UPDATE students 
SET email = 'newemail@edu.com' 
WHERE student_id = 101;

-- Update multiple records
UPDATE students 
SET status = 'inactive' 
WHERE age < 18;

-- Update with calculation
UPDATE enrollments 
SET grade = 'A' 
WHERE student_id = 101 AND course_id = 'CSE101';
```

#### **DELETE**

```sql
-- Delete specific records
DELETE FROM students WHERE student_id = 101;

-- Delete with condition
DELETE FROM enrollments WHERE grade IS NULL;

-- Delete all records (use with caution!)
DELETE FROM temp_students;
```

---

### 🔗 SQL Joins

```
┌──────────────────────────────────────────────┐
│              TYPES OF JOINS                   │
├──────────────────────────────────────────────┤
│                                               │
│  INNER JOIN    ──→ Matching records only     │
│  LEFT JOIN     ──→ All from left + matches   │
│  RIGHT JOIN    ──→ All from right + matches  │
│  FULL JOIN     ──→ All records from both     │
│  CROSS JOIN    ──→ Cartesian product         │
│  SELF JOIN     ──→ Table joined with itself  │
│                                               │
└──────────────────────────────────────────────┘
```

**Visual Representation:**

```
INNER JOIN          LEFT JOIN           RIGHT JOIN
   A ∩ B              A + (A ∩ B)         B + (A ∩ B)
   
   ┌───┐              ┌───┐              ┌───┐
   │ A │ B            │ A │ B            │ A │ B
   └───┘              └───┘              └───┘
     ▓                ▓▓▓▓                  ▓▓▓
```

#### **Join Examples**

```sql
-- INNER JOIN (only matching records)
SELECT s.name, c.title, e.grade
FROM students s
INNER JOIN enrollments e ON s.student_id = e.student_id
INNER JOIN courses c ON e.course_id = c.course_id;

-- LEFT JOIN (all students + their enrollments)
SELECT s.name, c.title, e.grade
FROM students s
LEFT JOIN enrollments e ON s.student_id = e.student_id
LEFT JOIN courses c ON e.course_id = c.course_id;

-- RIGHT JOIN (all courses + enrolled students)
SELECT s.name, c.title
FROM students s
RIGHT JOIN enrollments e ON s.student_id = e.student_id
RIGHT JOIN courses c ON e.course_id = c.course_id;

-- SELF JOIN (students in same batch)
SELECT s1.name AS student1, s2.name AS student2
FROM students s1
INNER JOIN students s2 ON s1.batch = s2.batch AND s1.student_id < s2.student_id;
```

---

### 🎯 Advanced SQL Concepts

#### **Subqueries**

```sql
-- Subquery in WHERE
SELECT name FROM students
WHERE student_id IN (
    SELECT student_id FROM enrollments WHERE grade = 'A'
);

-- Subquery in FROM
SELECT avg_grades.student_id, avg_grades.average
FROM (
    SELECT student_id, AVG(CAST(grade AS INT)) as average
    FROM enrollments
    GROUP BY student_id
) AS avg_grades
WHERE avg_grades.average > 80;

-- Correlated subquery
SELECT s.name
FROM students s
WHERE EXISTS (
    SELECT 1 FROM enrollments e 
    WHERE e.student_id = s.student_id AND e.grade = 'A'
);
```

#### **Views**

```sql
-- Create view
CREATE VIEW active_students AS
SELECT student_id, name, email
FROM students
WHERE status = 'active';

-- Use view
SELECT * FROM active_students;

-- Drop view
DROP VIEW active_students;
```

#### **Indexes**

```sql
-- Create index
CREATE INDEX idx_student_name ON students(name);

-- Composite index
CREATE INDEX idx_enrollment ON enrollments(student_id, course_id);

-- Unique index
CREATE UNIQUE INDEX idx_email ON students(email);

-- Drop index
DROP INDEX idx_student_name ON students;
```

#### **Transactions**

```sql
-- Start transaction
START TRANSACTION;

UPDATE accounts SET balance = balance - 1000 WHERE account_id = 101;
UPDATE accounts SET balance = balance + 1000 WHERE account_id = 102;

-- If all operations successful
COMMIT;

-- If error occurs
ROLLBACK;

-- Savepoint
SAVEPOINT sp1;
-- ... operations
ROLLBACK TO sp1;
```

---

### 📋 SQL Interview Questions Quick Reference

```
┌──────────────────────────────────────────────┐
│       TOP SQL INTERVIEW TOPICS               │
├──────────────────────────────────────────────┤
│                                               │
│ ✓ Difference: DELETE vs TRUNCATE vs DROP    │
│ ✓ Types of JOINS with examples              │
│ ✓ Primary Key vs Foreign Key                │
│ ✓ GROUP BY vs HAVING                         │
│ ✓ Subqueries vs Joins                        │
│ ✓ Normalization (1NF, 2NF, 3NF)             │
│ ✓ ACID properties                            │
│ ✓ Indexes and their types                   │
│ ✓ Views and their advantages                │
│ ✓ SQL injection and prevention              │
│                                               │
└──────────────────────────────────────────────┘
```

---

## 8. MongoDB (NoSQL)

### 🍃 What is MongoDB?

**MongoDB** is a NoSQL, document-oriented database that stores data as JSON-like documents (BSON) instead of tables and rows.

```
┌──────────────────────────────────────────────┐
│        RDBMS vs MongoDB Architecture          │
├──────────────────────────────────────────────┤
│                                               │
│  RDBMS                    MongoDB            │
│  ┌─────────┐             ┌──────────┐       │
│  │Database │             │ Database │       │
│  │    ↓    │             │    ↓     │       │
│  │ Tables  │             │Collections│      │
│  │    ↓    │             │    ↓     │       │
│  │  Rows   │             │Documents │       │
│  │    ↓    │             │    ↓     │       │
│  │ Columns │             │  Fields  │       │
│  └─────────┘             └──────────┘       │
│                                               │
└──────────────────────────────────────────────┘
```

---

### 🔹 Key Features of MongoDB

| Feature | Description | Benefit |
|---------|-------------|---------|
| **Document-Oriented** | Data stored as JSON/BSON documents | Flexible, intuitive |
| **Schema-less** | No fixed schema required | Easy to modify structure |
| **Scalability** | Horizontal scaling via sharding | Handles massive data |
| **Replication** | Replica sets for fault tolerance | High availability |
| **Indexing** | Multiple index types supported | Fast queries |
| **Aggregation** | Powerful data processing pipeline | Complex analytics |
| **High Performance** | Optimized read/write operations | Speed |

---

### 💾 MongoDB Document Example

```json
{
  "_id": ObjectId("652abf1234567890abcdef"),
  "student_id": 101,
  "name": "Asha Singh",
  "email": "asha@uni.edu",
  "dob": "2003-04-12",
  "age": 21,
  "status": "active",
  "address": {
    "street": "123 Main St",
    "city": "Mumbai",
    "state": "Maharashtra",
    "pincode": "400001"
  },
  "phone": ["+91-9876543210", "+91-9123456789"],
  "courses": [
    {
      "course_id": "CSE101",
      "title": "Database Systems",
      "credits": 4,
      "grade": "A",
      "semester": "Fall 2024"
    },
    {
      "course_id": "CSE102",
      "title": "Computer Networks",
      "credits": 3,
      "grade": "B+",
      "semester": "Fall 2024"
    }
  ],
  "created_at": ISODate("2024-09-30T10:00:00Z"),
  "updated_at": ISODate("2024-10-07T15:30:00Z")
}
```

**Key Points:**
- **Embedded Documents**: `address`, `courses` nested within main document
- **Arrays**: `phone`, `courses` can store multiple values
- **Flexible Schema**: Documents can have different fields
- **ObjectId**: Auto-generated unique identifier

---

### ⚖️ RDBMS vs MongoDB Detailed Comparison

| Aspect | RDBMS | MongoDB (NoSQL) |
|--------|-------|----------------|
| **Data Model** | Tables with rows & columns | Collections with documents |
| **Schema** | Fixed, predefined schema | Dynamic, flexible schema |
| **Relationships** | Foreign keys, JOIN operations | Embedded documents, references |
| **Transactions** | Full ACID compliance | Multi-document ACID transactions (4.0+) |
| **Scalability** | Vertical (upgrade hardware) | Horizontal (add more servers) |
| **Query Language** | SQL | MongoDB Query Language (MQL) |
| **Data Structure** | Structured data | Semi-structured / unstructured |
| **Joins** | Complex joins supported | Limited joins (prefer embedding) |
| **Performance** | Optimized for complex queries | Optimized for read/write speed |
| **Use Case** | Banking, ERP, transactional systems | Real-time analytics, IoT, content management |
| **Data Integrity** | Strong through constraints | Application-level validation |
| **Learning Curve** | Steeper (SQL syntax) | Easier (JSON-like) |

---

### 📚 MongoDB Terminology

```
┌────────────────────────────────────────────────┐
│      RDBMS ←→ MongoDB Terminology Mapping      │
├────────────────────────────────────────────────┤
│                                                 │
│  Database        →        Database             │
│  Table           →        Collection           │
│  Row/Record      →        Document             │
│  Column          →        Field                │
│  Index           →        Index                │
│  JOIN            →        Embedded/$lookup     │
│  Primary Key     →        _id (auto-generated) │
│  Foreign Key     →        Reference            │
│                                                 │
└────────────────────────────────────────────────┘
```

---

### 🛠️ MongoDB Operations

#### **Create (Insert)**

```javascript
// Insert single document
db.students.insertOne({
    student_id: 101,
    name: "John Doe",
    email: "john@edu.com",
    age: 24,
    courses: ["CSE101", "CSE102"]
});

// Insert multiple documents
db.students.insertMany([
    { student_id: 102, name: "Mary Smith", age: 25 },
    { student_id: 103, name: "Robert Johnson", age: 23 }
]);
```

#### **Read (Find)**

```javascript
// Find all documents
db.students.find();

// Find with condition
db.students.find({ age: { $gt: 22 } });

// Find one document
db.students.findOne({ student_id: 101 });

// Projection (select specific fields)
db.students.find({}, { name: 1, email: 1, _id: 0 });

// Sorting
db.students.find().sort({ age: -1 });

// Limiting results
db.students.find().limit(10);

// Count documents
db.students.countDocuments({ status: "active" });
```

#### **Update**

```javascript
// Update single document
db.students.updateOne(
    { student_id: 101 },
    { $set: { email: "newemail@edu.com" } }
);

// Update multiple documents
db.students.updateMany(
    { age: { $lt: 18 } },
    { $set: { status: "minor" } }
);

// Add to array
db.students.updateOne(
    { student_id: 101 },
    { $push: { courses: "CSE103" } }
);

// Increment value
db.students.updateOne(
    { student_id: 101 },
    { $inc: { age: 1 } }
);
```

#### **Delete**

```javascript
// Delete single document
db.students.deleteOne({ student_id: 101 });

// Delete multiple documents
db.students.deleteMany({ status: "inactive" });

// Delete all documents
db.students.deleteMany({});
```

---

### 🔍 MongoDB Query Operators

| Category | Operator | Description | Example |
|----------|----------|-------------|---------|
| **Comparison** | `$eq`, `$ne`, `$gt`, `$gte`, `$lt`, `$lte` | Compare values | `{ age: { $gte: 18 } }` |
| **Logical** | `$and`, `$or`, `$not`, `$nor` | Logical operations | `{ $or: [{ age: 18 }, { status: "active" }] }` |
| **Element** | `$exists`, `$type` | Field existence/type | `{ phone: { $exists: true } }` |
| **Array** | `$in`, `$nin`, `$all`, `$size` | Array operations | `{ courses: { $in: ["CSE101", "CSE102"] } }` |
| **Update** | `$set`, `$unset`, `$inc`, `$push`, `$pull` | Modify documents | `{ $set: { name: "New Name" } }` |

---

### 📊 MongoDB Aggregation Pipeline

```javascript
db.enrollments.aggregate([
    // Stage 1: Match documents
    { $match: { semester: "Fall 2024" } },
    
    // Stage 2: Group by student
    { 
        $group: {
            _id: "$student_id",
            avgGrade: { $avg: "$grade" },
            courseCount: { $sum: 1 }
        }
    },
    
    // Stage 3: Sort by average grade
    { $sort: { avgGrade: -1 } },
    
    // Stage 4: Limit results
    { $limit: 10 }
]);
```

**Common Pipeline Stages:**
- `$match` - Filter documents
- `$group` - Group by field and aggregate
- `$sort` - Sort documents
- `$project` - Select specific fields
- `$limit` - Limit number of results
- `$skip` - Skip documents
- `$lookup` - Left outer join
- `$unwind` - Deconst
│